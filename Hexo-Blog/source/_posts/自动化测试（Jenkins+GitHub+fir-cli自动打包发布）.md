---
title: 自动化测试（Jenkins+GitHub+fir-cli自动打包发布）
date: 2017-02-19
categories: [自动化测试]
---
* 
{% codeblock lang:sh %}
    目录：
    1、安装Jenkins
    2、进入Jenkins
    3、下载安装firm.im插件
    4、安装GitHub和Git插件
    5、安装Xcode插件
    6、创建并配置项目
    7、常见错误
{% endcodeblock %}

终端启动Jenkins并启动Jenkins
cd /Applications/Jenkins/
java -jar jenkins.war --httpPort=7070

### 1、安装Jenkins  
方式一：官网下载 {% link Jenkins https://jenkins.io/ %}

* 安装完后通过终端打开
{% codeblock lang:sh %}
    $ open /Users/ZZX/Desktop/Jenkins/jenkins.war
{% endcodeblock %}

方式二：通过命令行下载安装
* 首先安装homebrew
{% codeblock lang:sh %}
    $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endcodeblock %}

* 然后安装Jenkins
{% codeblock lang:sh %}
    $ brew install jenkins 
{% endcodeblock %}

### 2、进入Jenkins  
在浏览器里面输入 http://localhost:8080/
当端口发送冲突时，在终端输入一下命令修改端口
* 
{% codeblock lang:sh %}
    $cd /Applications/Jenkins/
    $ java -jar jenkins.war --httpPort=7070
{% endcodeblock %}


### 3、下载和安装fir.im的插件  
* 下载 {% link 插件 http://7xju1s.com1.z0.glb.clouddn.com/fir-plugin(5).hpi %}
* 安装插件
进入Jenkins点击左上方的系统管理然后进入插件管理
![image](http://129.204.47.207/img/blog/20161211-1-1.png)

然后点高级
![image](http://129.204.47.207/img/blog/20161211-1-2.png)


往下拖找到上传插件
![image](http://129.204.47.207/img/blog/20161211-1-3.png)



把下载好的文件传入，然后等待安装完成
![image](http://129.204.47.207/img/blog/20161211-1-4.png)




### 4、安装GitHub和Git插件  
为了能够在GitHub分支更新后能够自动打包上传，这里需要安装两个git插件
* GitHub Plugin
* Git Plugin

### 5、安装Xcode插件  
* Xcode integration

6、创建并配置项目  
* 6.1、创建一个新的项目 

* 6.2、项目基本信息  
![image](http://129.204.47.207/img/blog/20161211-1-5.png)


* 6.3、源码管理信息   
![image](http://129.204.47.207/img/blog/20161211-1-6.png)


* 6.4、构建触发器：  
![image](http://129.204.47.207/img/blog/20161211-1-7.png)



* 6.5、构建->添加构建步骤  
![image](http://129.204.47.207/img/blog/20161211-1-8.png)


* 6.6、构建->Xcode  
![image](http://129.204.47.207/img/blog/20161211-1-9.png)


* 6.7、构建->Xcode证书信息  
我这里是git上的项目已经配置好了证书和provision profiles。
1、勾选Unlock Keychain；
2、Keychain path中输入 ￥{HOME}/Library/Keychains/login.keychain;
3、Keychain password为你的钥匙串密码。
![image](http://129.204.47.207/img/blog/20161211-1-10.png)



* 6.8、构建后操作->Upload to fir.im  
安装过fim.im插件以后，这里就可以看到Upload to fim.im选项了，fim.im的上传脚本可参考fir.im的相关文档进行操作，如果使用的是蒲公英或者其他第三方托管平台，这一步的操作是一样的，安装插件-添加构建后操作->添加上传脚本
![image](http://129.204.47.207/img/blog/20161211-1-11.png)
![image](http://129.204.47.207/img/blog/20161211-1-12.png)


至此，所有的配置都已经完成，下面就可以进行构建操作了。

最后一步：构建生成ipa文件，并上传fir.im
回到刚刚创建的项目，进入项目页面，点击左边的“立即构建”按钮，即可开始构建。构建完成以后，可以在配置的ipa所在的路径查看是否生产ipa文件。登录fir.im查看是否已经上传到fim.im上。至此，本教程结束。
![image](http://129.204.47.207/img/blog/20161211-1-13.png)




------------------------------------------------------
2016.06.27补充
如果项目中使用了cocoapods管理第三方框架，那么构建->xcode设置时需要注意：

1. 因为项目中使用 workspace， 所以 Target 可以不填。
![image](http://129.204.47.207/img/blog/20161211-1-14.png)


    Xcode Schema File:  这里的 Laomoney_debug 就是我在 Xcode 项目中新建的 scheme。
    Xcode Workspace File: 使用cocoaPods的项目包含有 workspace，这里设置对应路径，注意不需要带上 .xcworkspace 后缀。
    XcodeProjectDirectory: Xcode 项目所在目录
    Xcode Project File: Xcode 项目文件，这里需要带上 .xcodeproj 后缀。
    Build output directory: 设置构建输出目录。



![image](http://129.204.47.207/img/blog/20161211-1-15.png)


以下是项目的文件夹目录

![image](http://129.204.47.207/img/blog/20161211-1-16.png)



### 7、常见错误  
1、错误：rsync error: some files could not be transferred (code 23) at /BuildRoot/Library/Caches/com.apple.xbs/Sources/rsync/rsync-47/rsync/main.c(992) [sender=2.6.9]
Command /bin/sh failed with exit code 23
报如上错误时，貌似是一堆图片资源冲突。
解决方法：
将 ~/Library/Developer/Xcode/DerivedData/ 目录下的工程缓冲删除掉即可。




