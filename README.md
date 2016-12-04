# shenjianshou_spiders
基于神箭手云爬虫平台的简单例子

> 既然你来到了这里，想必你肯定已经知道了神箭手云爬虫平台是干什么的，目的也是非常的明确。
> 那么接下来的过程中，我将给你演示如何在最快时间内编写一个简单的爬虫，每一个属性的讲解，将会让你一路顺风。

demo项目GitHub地址：




--------------------------------
####**进入爬虫市场**


首先进入爬虫市场，登录，链接在这--[爬虫市场](http://www.shenjianshou.cn/index.php?r=market/index)。
![这里写图片描述](http://img.blog.csdn.net/20161204233506561)

在这里也可以使用别人的爬虫和API，但是这不是我们的目的，况且博主自己把大部分爬虫看了，很少会有人将爬虫的代码开源出来，只有去官方的[GitHub](https://github.com/ShenJianShou/crawler_samples)能看到几个例子，但是对于初学者来说，还是稍微难了一点。

这里是神箭手的开发文档，如果你想真的写爬虫，最好还是先过一遍文档，链接在这--[开发文档](http://docs.shenjianshou.cn/)。

第一遍应该能了解个大概，但是又不知从何入手，没关系，主题来了。

--------------------------------
####**创建爬虫**

![这里写图片描述](http://img.blog.csdn.net/20161204233549609)

进入我的控制台或者我的爬虫，点击新建应用。

然后弹窗中选择自己开发，输入名字，点击创建。

进入到项目中。

--------------------------------
####**编辑代码**

这里是我一个采集名叫牛人微信的一个小网站。


```
var configs = {
  domains: ["weixin.niurenqushi.com"],
  //定义爬虫爬取哪些域名下的网页, 非域名下的url会被忽略以提高爬取速度
  
  scanUrls: ["http://weixin.niurenqushi.com/"],
  //定义爬虫的入口链接, 爬虫从这些链接开始爬同时这些链接也是监控爬虫所要监控的链接
  
  contentUrlRegexs: "http://weixin\\.niurenqushi\\.com/article/list\\-\\d+.html",
  //定义”内容页”url的规则“内容页”是指包含要爬取内容的网页, 比如,“http://www.qiushibaike.com/article/117844937“就是糗事百科一个”内容页”
  
  helperUrlRegexes: ["http://weixin\\.niurenqushi\\.com/article/2016-11-30/\\d+.html"],
  //定义”列表页”url的规则对于有列表页的网站, 使用此配置可以大幅提高爬虫的爬取速率“列表页”是指包含”内容页”列表的网页, 比如,“http://www.qiushibaike.com/8hr/page/2/?s=4867046“就是糗事百科的一个”列表页”
  
  enableJS: false,
  //是否使用JS渲染默认值是false, 如果需要使用JS渲染, 可以设置此项为true
  
  interval: 3000,
  //爬虫爬取每个网页的时间间隔单位: 毫秒
  
  fields: [
  //定义”内容页”的抽取规则规则由一个个field组成, 一个field代表一个数据抽取项
    {
        name: "article_title",//名称字段，可以随便取
        selector: "//div[contains(@class,'contitle')]/h1",//指的是你要抓取的内容在哪个标签中，这里就是在一个名叫contitle的div中的h1中抓取内容
        required: false//是否能为空
    },
    {
       name: "article_content",
       selector: "//div[contains(@id,'contentbody')]",
       required: false
    },
    {
       name: "article_publish_time",
       selector: "//div[contains(@class,'contitle')]//div",
      required: false
    },
     {
       name: "article_topic",
       selector: "//a[contains(@class,'ly')]",
      required: false
    }
  ]
};

//下面这个方法，当一个field的内容被抽取到后进行的回调, 在此回调中可以对网页中抽取的内容作进一步处理
configs.afterExtractField = function(fieldName, data, page){
  if (fieldName == "article_content") {
        return cacheImg(data); // 返回可被托管到图片云服务器上的url，如果你只想将数据保存在本地，那么这个可以不写。
    }
	 if(fieldName=="article_publish_time"){
      data = Date.parse(new Date())/1000+"";//将抓取到的时间转换成2016-12-4形式
    }
  return data;
};
  
var crawler = new Crawler(configs);
crawler.start();//开启爬虫
```

可以在右边测试栏先测试。

####**抓取结果**

![这里写图片描述](http://img.blog.csdn.net/20161204233643753)
点击左侧总览，然后右上角启动。

稍作等待。

点击左侧爬取结果。

####**发布结果**
不论你是想发布到网站上还是保存数据下来，平台都有方法。

如果想要导出Excel表格形式，点击左侧导出到文件。按需求选择，点击生成文件即可。

如果是想发布到网站上，点击这里，会有很好的解释。--[数据发布](http://docs.shenjianshou.cn/use/datapub/useDataPublish.html)

这里有很多集成式网站的接口，可以直接使用，博主就是用的wecenter发布的数据，www.nicesunny.com，网站没啥东西。

如果在发布过程后，数据被发布了，但是其中的图片没有显示出来，那么可以试试神箭手平台的图片托管，有三种，阿里，七牛，神箭手，为了方便，我用的神箭手。

[如何将图片托管到神箭手?](http://docs.shenjianshou.cn/use/picture/useSJSPhotoStorage.html)
![这里写图片描述](http://docs.shenjianshou.cn/images/publish/use_sjs_photo_storage/use_sjs_photo_storage_img_12.jpg)

> 好了，如果有其他的问题，随时可以联系博主，邮箱1178539345@qq.com。
> 
> 如果喜欢的话，请在GitHub上给上一颗star吧！
