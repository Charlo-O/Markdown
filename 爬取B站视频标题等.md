# 爬取B站视频标题等
终于来了点高端的了

去b站爬取视频标题 

不过看原作者的方法

```
from selenium import webdriver
from selenium.common.exceptions import TimeoutException
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from bs4 import BeautifulSoup
import xlwt


browser = webdriver.PhantomJS()
WAIT = WebDriverWait(browser, 10)
browser.set_window_size(1400,900)


book=xlwt.Workbook(encoding='utf-8',style_compression=0)

sheet=book.add_sheet('蔡徐坤篮球',cell_overwrite_ok=True)
sheet.write(0,0,'名称')
sheet.write(0,1,'地址')
sheet.write(0,2,'描述')
sheet.write(0,3,'观看次数')
sheet.write(0,4,'弹幕数')
sheet.write(0,5,'发布时间')

n=1

def search():

    try:
        print('开始访问b站....')
        browser.get("https://www.bilibili.com/")

        # 被那个破登录遮住了
        index = WAIT.until(EC.element_to_be_clickable((By.CSS_SELECTOR, "#primary_menu > ul > li.home > a")))
        index.click()

        input = WAIT.until(EC.presence_of_element_located((By.CSS_SELECTOR, "#banner_link > div > div > form > input")))
        submit = WAIT.until(EC.element_to_be_clickable((By.XPATH, '//*[@id="banner_link"]/div/div/form/button')))

        input.send_keys('蔡徐坤 篮球')
        submit.click()

        # 跳转到新的窗口
        print('跳转到新窗口')
        all_h = browser.window_handles
        browser.switch_to.window(all_h[1])

        get_source()
        total = WAIT.until(EC.presence_of_element_located((By.CSS_SELECTOR, "#server-search-app > div.contain > div.body-contain > div > div.page-wrap > div > ul > li.page-item.last > button")))
        return int(total.text)
    except TimeoutException:
        return search()


def next_page(page_num):
    try:
        print('获取下一页数据')
        next_btn = WAIT.until(EC.element_to_be_clickable((By.CSS_SELECTOR, '#server-search-app > div.contain > div.body-contain > div > div.page-wrap > div > ul > li.page-item.next > button')))
        next_btn.click()
        WAIT.until(EC.text_to_be_present_in_element((By.CSS_SELECTOR, '#server-search-app > div.contain > div.body-contain > div > div.page-wrap > div > ul > li.page-item.active > button'),str(page_num)))
        get_source()
    except TimeoutException:
        browser.refresh()
        return next_page(page_num)


def save_to_excel(soup):
    list = soup.find(class_='all-contain').find_all(class_='info')

    for item in list:
        item_title = item.find('a').get('title')
        item_link = item.find('a').get('href')
        item_dec = item.find(class_='des hide').text
        item_view = item.find(class_='so-icon watch-num').text
        item_biubiu = item.find(class_='so-icon hide').text
        item_date = item.find(class_='so-icon time').text

        print('爬取：' + item_title)

        global n

        sheet.write(n, 0, item_title)
        sheet.write(n, 1, item_link)
        sheet.write(n, 2, item_dec)
        sheet.write(n, 3, item_view)
        sheet.write(n, 4, item_biubiu)
        sheet.write(n, 5, item_date)

        n = n + 1


def get_source():
    WAIT.until(EC.presence_of_element_located((By.CSS_SELECTOR,'#server-search-app > div.contain > div.body-contain > div > div.result-wrap.clearfix')))
    html = browser.page_source
    soup = BeautifulSoup(html,'lxml')
    save_to_excel(soup)

def main():

    try:
        total = search()
        print(total)

        for i in range(2,int(total+1)):
            next_page(i)

    finally:
        browser.close()


if __name__ == '__main__':
    main()
    book.save(u'蔡徐坤篮球.xlsx')
```
直接 CV ！结果：
![](https://user-gold-cdn.xitu.io/2019/7/14/16bf0d00592fe5d6?w=391&h=47&f=png&s=3386)

我的尿性，能不改的地方就不看

问题出在哪？仔细看文章，知道是要用

selenium + phantomjs

昨天倒是把 selenium 下载了 ，但是 phantomjs 下载速度实在太慢

![](https://user-gold-cdn.xitu.io/2019/7/14/16bf0ce86fb1fdc5?w=236&h=53&f=png&s=3435)

我的 ss 和 ssr 似乎都无济于事 

于是我又达到了万能的百度 

原谅我虽然百度一生黑

关键时刻还是习惯用百度 google 搞出来英文居多

本来就被 bug 搞得焦头烂额

还要再动脑子 实在头疼

结果发现

![](https://user-gold-cdn.xitu.io/2019/7/14/16bf0d45b43e6c57?w=575&h=141&f=png&s=34342)

![](https://user-gold-cdn.xitu.io/2019/7/14/16bf0d4ff0ae62e5?w=660&h=230&f=png&s=41567)

终于知道那是个啥了 

于是机智的我直接把 phantomjs 改成 Chrome 了 C 大写！

运行！

![](https://user-gold-cdn.xitu.io/2019/7/14/16bf0d8956435302?w=662&h=342&f=png&s=94313)

完美！
![](https://user-gold-cdn.xitu.io/2019/7/14/16bf0d945f63277c?w=448&h=396&f=png&s=31965)
但是表放哪了啊？

目录也没有，于是直接搜了，，

就在根目录里面

![](https://user-gold-cdn.xitu.io/2019/7/14/16bf0db44c6594bb?w=522&h=459&f=png&s=61784)

原谅我这不羁的文件管理

打开看看成果

![](https://user-gold-cdn.xitu.io/2019/7/14/16bf0e41a5b5267e?w=771&h=131&f=png&s=11167)

不急，记得上次说过，用记事本打开字符就好了。。

![](https://user-gold-cdn.xitu.io/2019/7/14/16bf0e562a5f8a84?w=944&h=706&f=png&s=65754)

有点不大对劲啊。

![](https://user-gold-cdn.xitu.io/2019/7/14/16bf0e5f9ae61d9f?w=379&h=137&f=png&s=6430)

果然没用，咋办，，

继续百度！

方法很简单，改成 .xls 就好了。

![](https://user-gold-cdn.xitu.io/2019/7/14/16bf0e7dfd687bcd?w=1099&h=595&f=png&s=215536)

哈哈，也不是很难嘛~~

```
from selenium import webdriver
from selenium.common.exceptions import TimeoutException
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from bs4 import BeautifulSoup
import xlwt


browser = webdriver.Chrome()
WAIT = WebDriverWait(browser, 10)
browser.set_window_size(1400,900)


book=xlwt.Workbook(encoding='utf-8',style_compression=0)

sheet=book.add_sheet('python',cell_overwrite_ok=True)
sheet.write(0,0,'名称')
sheet.write(0,1,'地址')
sheet.write(0,2,'描述')
sheet.write(0,3,'观看次数')
sheet.write(0,4,'弹幕数')
sheet.write(0,5,'发布时间')

n=1

def search():

    try:
        print('开始访问b站....')
        browser.get("https://www.bilibili.com/")

        # 被那个破登录遮住了
        index = WAIT.until(EC.element_to_be_clickable((By.CSS_SELECTOR, "#primary_menu > ul > li.home > a")))
        index.click()

        input = WAIT.until(EC.presence_of_element_located((By.CSS_SELECTOR, "#banner_link > div > div > form > input")))
        submit = WAIT.until(EC.element_to_be_clickable((By.XPATH, '//*[@id="banner_link"]/div/div/form/button')))

        input.send_keys('python')
        submit.click()

        # 跳转到新的窗口
        print('跳转到新窗口')
        all_h = browser.window_handles
        browser.switch_to.window(all_h[1])

        get_source()
        total = WAIT.until(EC.presence_of_element_located((By.CSS_SELECTOR, "#server-search-app > div.contain > div.body-contain > div > div.page-wrap > div > ul > li.page-item.last > button")))
        return int(total.text)
    except TimeoutException:
        return search()


def next_page(page_num):
    try:
        print('获取下一页数据')
        next_btn = WAIT.until(EC.element_to_be_clickable((By.CSS_SELECTOR, '#server-search-app > div.contain > div.body-contain > div > div.page-wrap > div > ul > li.page-item.next > button')))
        next_btn.click()
        WAIT.until(EC.text_to_be_present_in_element((By.CSS_SELECTOR, '#server-search-app > div.contain > div.body-contain > div > div.page-wrap > div > ul > li.page-item.active > button'),str(page_num)))
        get_source()
    except TimeoutException:
        browser.refresh()
        return next_page(page_num)


def save_to_excel(soup):
    list = soup.find(class_='all-contain').find_all(class_='info')

    for item in list:
        item_title = item.find('a').get('title')
        item_link = item.find('a').get('href')
        item_dec = item.find(class_='des hide').text
        item_view = item.find(class_='so-icon watch-num').text
        item_biubiu = item.find(class_='so-icon hide').text
        item_date = item.find(class_='so-icon time').text

        print('爬取：' + item_title)

        global n

        sheet.write(n, 0, item_title)
        sheet.write(n, 1, item_link)
        sheet.write(n, 2, item_dec)
        sheet.write(n, 3, item_view)
        sheet.write(n, 4, item_biubiu)
        sheet.write(n, 5, item_date)

        n = n + 1


def get_source():
    WAIT.until(EC.presence_of_element_located((By.CSS_SELECTOR,'#server-search-app > div.contain > div.body-contain > div > div.result-wrap.clearfix')))
    html = browser.page_source
    soup = BeautifulSoup(html,'lxml')
    save_to_excel(soup)

def main():

    try:
        total = search()
        print(total)

        for i in range(2,int(total+1)):
            next_page(i)

    finally:
        browser.close()


if __name__ == '__main__':
    main()
    book.save(u'python.xls')

```

附上我的山寨代码，爬取python 

哈哈~



