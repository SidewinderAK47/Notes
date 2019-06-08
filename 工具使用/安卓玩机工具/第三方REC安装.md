# [[教程\]](http://www.miui.com/forum.php?mod=forumdisplay&fid=483&filter=typeid&typeid=3354) [Windows下如何下载官方adb工具包并刷入第三方rec~~~](http://www.miui.com/thread-9117980-1-1.html)

# 玩机工具集

1.SuperSU

2.adb

3.[twrp](https://dl.twrp.me/cancro/twrp-3.1.0-0-cancro.img)

## Windows下如何下载官方adb工具包并刷入第三方rec~~~

  **一、什么是Adb**    

​    Adb的全称为Android Debug Bridge，是一款强大的工具，通过它，我们能够干很多事情，例如安装/卸载软件、查看手机端运行日志、重启手机、运行shell命令等。但对于我们这些非开发者而言，最常用的功能莫过于安装第三方recovery了。

**二、如何下载官方最新Adb工具包**


​         搜索引擎输入Adb工具包，会弹出非常多的下载页面，但是小伙伴们使用adb时有时会弹出错误提示，原因往往是由于adb版本过低，下载最新的Adb工具包能够避免这一问题。

​      1、浏览器进入 [https://developer.android.google ... platform-tools.html](https://developer.android.google.cn/studio/releases/platform-tools.html)，会弹出以下界面，箭头所指就是我们需要下载的windows平台下的adb工具包。

​      ![img](https://attach.bbs.miui.com/forum/201707/25/163401olhzu8g1911l8akk.jpg.thumb.jpg)

​    2、同意相关条款

![img](https://attach.bbs.miui.com/forum/201707/25/164009qazkz9vvv9no9vav.jpg.thumb.jpg)





![img](https://attach.bbs.miui.com/forum/201707/25/164302x5wzu4s5p5dyhuem.jpg.thumb.jpg)



  3、下载/保存

![img](https://attach.bbs.miui.com/forum/201707/25/210054xlk3l9k5pkpnfzzl.jpg.thumb.jpg)

​     除了上述步骤，我们也可以通过直链来下载 [https://dl.google.com/android/re ... -latest-windows.zip](https://dl.google.com/android/repository/platform-tools-latest-windows.zip)  

​    这里提供一个目前（2017.07.25）最新的的Adb工具包 ![img](第三方REC安装.assets/zip.png)[platform-tools-latest-windows.zip](http://www.miui.com/forum.php?mod=attachment&aid=MTIxMjMyNzV8NWU3Yzc1NGV8MTU1OTk5MDY2OXwwfDkxMTc5ODA%3D) *(7.16 MB, 下载次数: 14836)* 

**三、安装第三方recovery**

​       1、下载第三方rec，如twrp，这里我只演示原版twrp的下载，汉化版请各位利用百度或者论坛的搜索功能自行寻找

​           浏览器输入 <https://twrp.me/Devices> ，然后选择自己的机型，我是小米4，机型代号米3米4相同，选择cancro，其他机型请选择自己机型对应的代号


![img](https://attach.bbs.miui.com/forum/201707/25/210550w2uynnioiablphhb.jpg.thumb.jpg)

以下就是目前twrp原版支持的所有小米型号    

![img](https://attach.bbs.miui.com/forum/201707/25/210946rpkdsqdzpiaagji2.jpg.thumb.jpg) 

点进去后，按下图箭头所示操作，也可选择方框下的另一个下载站点，具体选择哪个根据下载速度来决定

![img](https://attach.bbs.miui.com/forum/201707/25/212852j62kg771zv7tvt1t.jpg.thumb.jpg) 

下图框中是twrp的最新和历史版本，一般选择最新版本下载


![img](https://attach.bbs.miui.com/forum/201707/25/213441ree63bvvb6564srb.jpg.thumb.jpg)

点击后会出现如下页面，下载即可

![img](https://attach.bbs.miui.com/forum/201707/25/213657d42jr1ypdxedsy9l.jpg.thumb.jpg)

   2、解压先前下载的adb工具包到任意目录，如D:\ADB

![img](https://attach.bbs.miui.com/forum/201707/25/220442w6zcqykqz02hflet.jpg.thumb.jpg)

3、将步骤1下载的twrp移入adb工具包文件夹内，并重命名为 recovery，注意，recovery不能打错，打错后使用批处理命令刷入时会报错

![img](https://attach.bbs.miui.com/forum/201707/25/220717xu4s6futs6fw4w1z.jpg.thumb.jpg) 

![img](https://attach.bbs.miui.com/forum/201707/25/221047xu6u0s7r84rszl74.jpg.thumb.jpg)

4.有bl锁的机型，请前往 <http://www.miui.com/unlock/index.html>  解锁，官方解锁说明<http://www.miui.com/thread-3367802-1-1.html>，解锁完成后才能刷入第三方rec， 
5.将我提供的批处理命令移入，双击该命令，按提示操作即可，批处理命令 ： ![img](http://www.miui.com/static/image/miui/base/filetype/zip.png)[将解压后的文件移入adb工具包目录.zip](http://www.miui.com/forum.php?mod=attachment&aid=MTIxMjc4NTd8OWNhZTkyNTF8MTU1OTk5MDY2OXwwfDkxMTc5ODA%3D) *(499 Bytes, 下载次数: 2664)* 

![img](https://attach.bbs.miui.com/forum/201707/25/221634lejeqejr0663j2jd.jpg.thumb.jpg)

![img](https://attach.bbs.miui.com/forum/201707/25/222048bvshntxlt3leuu8v.jpg.thumb.jpg)

![img](https://attach.bbs.miui.com/forum/201707/25/223005f1gmzs6zgg6h6zsg.jpg.thumb.jpg)



说明： 1. 写这个帖子是为了方便某些坚持使用稳定版MIUI，不想频繁的更新系统（开发板）， 又想获取root的朋友，刷入第三方rec后只需要rec下卡刷supersu就能获取完整root，具体教程请自行搜索
           \2. 也可通过上面给出的方法刷回官方rec，rec可从官方线刷包中提取  