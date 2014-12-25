---
title: 基于kinect的检票系统
date: 2014-10-15 23:44:59
tags: [kinect,.Net]
desc: 

---

## 背景
		
我有一朋友经营个儿童游乐场，一般售票后会给顾客打印张小票，顾客当天内可凭票多次进出。这里有个漏洞：A购票后把小票给B，那B凭小票也可以进出游乐场了。如何解决这个问题？

这里使用个人的抓拍照片和票对应的方法。简单流程如下

	进入->拍照->录入小票和照片
					再次进入->扫描小票->查询对应照片
										   ->离开

最好拍照设备能自己工作。通过人物捕捉，当有目标进入镜头后自动拍照。想到动作捕捉，kinect太适合了，它生来就是做这个的。
![kinect1.0](https://cdn.yangguangxi.com/fm_1.jpg)
<!--more-->
## 分析需求

现在需要做有：
	
- 自动捕捉目标并拍照
- 将照片编号打印到热敏小票
- 能通过小票编号查照片
- 登记小票使用次数、时间

	把照片编号生成的条形码打印到热敏小票，查询小票时用条码扫描枪扫码即可，简直不要再简单。这样不需要增加人手，不需要学习新操作，甚至不用关心这个应用设备存在，就能解决问题。

## 施工阶段🚧

看了两本kinect指导书算是可行性分析了。尤其是[《Kinect应用开发实战》](http://book.douban.com/subject/20366360/)，里面丰富的例子就像定心丸。
![Kinect人机交互开发实践](https://cdn.yangguangxi.com/fm_2.jpg)

[《Kinect人机交互开发实践》](https://book.douban.com/subject/20423598/)比较简单，篇幅短。 

![Kinect应用开发实战](https://cdn.yangguangxi.com/fm_3.jpg)

[《Kinect应用开发实战》](http://book.douban.com/subject/20366360/)很好的入门书籍，讲的非常全面。技术、背景、NUI的发展历程及未来的展望都有作者清晰的可参考的观点。

平台和技术选择.NET WPF，好像也没得选😂。
将要完成的功能分为几个节点：

1. UI界面，能适应各种显示器的自适应布局（现学现用XAML）
 ![界面草图](https://cdn.yangguangxi.com/fm_ui.PNG)
 ![](https://cdn.yangguangxi.com/fm001.png)
2. 照片采集，摄像头可根据镜头内目标位置调整角度，自动拍摄）
![](https://cdn.yangguangxi.com/fm002.png)
3. 用定时任务清除过期照片（自动拍照会不小心拍到路人甲乙丙丁，手动可以选择需要的照片，提供配置照片最大阈值参数，超过配置的数量后照片自动清理旧照片。）
![](https://cdn.yangguangxi.com/fm003.png)
![](https://cdn.yangguangxi.com/fm004.png)
4. 打印编号生成的条形码到小票，调用热敏打印机切刀自动切票。
5. 登记小票信息到管理系统（建立小票-照片-人的关联。）
6. 扫码枪扫过小票条码后登记（数据不会特别多，用了Access数据库）。
![](https://cdn.yangguangxi.com/fm005.png)
![](https://cdn.yangguangxi.com/fm_end.jpg)

##最后
免费给他们Logo美化了（这货不是VS吗？）。

		修改前
![](https://cdn.yangguangxi.com/fm_logo_before.png)
		
		修改后		
![](https://cdn.yangguangxi.com/fm_logo_after.png)


