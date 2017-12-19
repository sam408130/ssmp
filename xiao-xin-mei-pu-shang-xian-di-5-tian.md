## 进度概况
小程序上线第六天，因为控制了砍价的门槛，用户增长进度名下下降了许多

![](/assets/屏幕快照 2017-12-19 下午8.22.02.png)

![](/assets/屏幕快照 2017-12-19 下午8.22.10.png)

这两条曲线很接近，说明每天访问的用户基本是新用户，小程序本身留存很低，这个目前没有启动用户回访策略有直接关系

![](/assets/屏幕快照 2017-12-19 下午8.24.46.png)

目前推送管理已经上线(刚哥的效率就是高)，配合着推送营销的签到红包也会在明天上线，明天应该会有一波用户回流高峰，需要安排好客服接客。

## 页面交互数据监测
为了更好的监测小程序使用过程中的交互问题，以及转化问题，我上线了页面主要功能的打点监测，这在微信后台的数据分析中事件管理里面可以查看：

![](/assets/屏幕快照 2017-12-19 下午8.29.52.png)

数据监测是从今天开始的，首先来看一条主要路径转化：

1. 用户参与砍价 -> 砍价成功页面点击去首页逛逛 -> 浏览首页 -> 首页点击商品

![](/assets/屏幕快照 2017-12-19 下午8.36.43.png)

这条转化路径描述的是新用户作为被分享者进入砍价页面后，帮别人砍价，进而对平台产生兴趣，点击回首页，并查看商品的【连续】操作转化路径。很遗憾，目前的准化率非常低。突出表现在用户帮别人看完价后，只有2.46%的人点击了【去首页逛逛】的按钮。问题已经很明显，砍价成功页的引导和诱惑还不够！

2.页面退出率也反应了落地页本身对用户的诱导性做的不够

 ![](/assets/屏幕快照 2017-12-19 下午8.47.26.png)
 
 上图中pages/kanDetail/kanDetail表示砍价详情页面，该页面作为入口页面在最近7天内有73438次，作为退出页有69759次。这说明两个问题：
   第一，砍价详情页面说小新美铺传递给新用户的落地页。
   第二，用户进入落地页完成砍价动作后就离开了。
 刚才第一条漏洞分析看的时用户砍完价后连续操作的转化路径，下面看一下进入砍价详情页后的人，又访问了首页的转化：
 
 ![](/assets/屏幕快照 2017-12-19 下午8.55.13.png)
 
 这是今天一天的数据（上午10点到晚上8点多），进入砍价页面的有1987人，但是这1987人中只有181人进入了首页。已经够说明问题了！
 
<span style="color:blue">some *blue* text</span>.