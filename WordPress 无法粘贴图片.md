WordPress 无法粘贴图片

自己的博客搭建起来也有好久了，了写了几篇文章在上面，不过比较多的是转载其他地方的。

连续发了几篇文章发现了一个之名的问题，如果你的文章是直接 CV 的，那么不好意思，wordpress 是不会给你显示出来的，就像这样：


![](https://user-gold-cdn.xitu.io/2019/7/21/16c13fcf547914fb?w=384&h=483&f=png&s=30606)

这可怎么搞？当让百度啊！

得到的解决办法也很简单，装插件！

插件正是个好东西，，chrome 插件， xposed 插件，wordpress 插件，有插件简直为所欲为！

但是，现实并没有想象的那么美好，查到了一个插件叫 Image Paste ,



最近一次更新已经是六年前了！果然没用了！


![](https://user-gold-cdn.xitu.io/2019/7/21/16c14016e72e2cad?w=754&h=719&f=png&s=63355)

不过我并不气馁，继续找，果然又给我找到了一款插件叫OnePress Image Elevator


![](https://user-gold-cdn.xitu.io/2019/7/21/16c14051005b5157?w=764&h=803&f=png&s=226906)

满怀希望，又让我失望！还是没用？

怎么办！怎么办！这他妈百度也没辙了啊！

不过我忽然想到了Markdown! 于是机制的我又去搜索了一下 markdown编辑的插件，果然有！叫做WP Githuber MD


![](https://user-gold-cdn.xitu.io/2019/7/21/16c140a869a32b12?w=763&h=780&f=png&s=227679)

编辑界面是这样的：


![](https://user-gold-cdn.xitu.io/2019/7/21/16c142267129575d?w=928&h=294&f=png&s=24999)

搞完用最近翻译的一篇文章实验。[敏捷也许是个问题](https://github.com/xitu/gold-miner/blob/master/TODO1/agile-agile-blah-blah.md)。成功！图片完美显示。

于是激动的再想把这篇 [【Python】爬取B站视频标题等](http://www.charlo.cn/2019/07/21/%e3%80%90python%e3%80%91%e7%88%ac%e5%8f%96b%e7%ab%99%e8%a7%86%e9%a2%91%e6%a0%87%e9%a2%98%e7%ad%89/) 也发布，不过失败了！

但仔细思考一下，估计因该是图床的问题。

wordpress 对国内的网站比较无力。

于是想到了把 md 上传到 git 上,这总该没问题了把。

说干就干，于是直接再 git 上新建个仓库。

![](https://user-gold-cdn.xitu.io/2019/7/21/16c141a8fa5c21b2?w=844&h=677&f=png&s=65084)

把 md 文件上传到仓库，打开后之后通过 F12 复制其 html 源码 


![](https://user-gold-cdn.xitu.io/2019/7/21/16c141d37118a683?w=588&h=222&f=png&s=43840)

再通过 html 转 md ,推荐这个[在线工具](https://tool.lu/markdown#)


![](https://user-gold-cdn.xitu.io/2019/7/21/16c141f59fc90db8?w=805&h=464&f=png&s=28774)

之后复制粘贴即可~


![](https://user-gold-cdn.xitu.io/2019/7/21/16c142082be9d5b3?w=905&h=125&f=png&s=27418)

图片地址显示为 git 了，之后再复制粘贴，一切正常！

有时候在想为什么又这么麻烦呢，直接放知乎，简书，掘金，git 不都可以么？可能是因为我的博客是花了钱的把，，

如果有更好好的办法，希望大师教教我。。




