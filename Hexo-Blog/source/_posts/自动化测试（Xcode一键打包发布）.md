---
title: 自动化测试（Xcode一键打包发布）
date: 2017-02-26
categories: [自动化测试]
---

在研究自动化打包的过程中，我们发现使用Mac系统提供的Automator可以进一步为一键打包提供便利，提高开发效率，效果如图。
![image](http://129.204.47.207/img/blog/20161211-2-1.png)

### Automator
Automator是苹果公司为他们的Mac OS X系统开发的一款软件。只要通过点击拖拽鼠标等操作就可以将一系列动作组合成一个工作流，从而帮助你自动的（可重复的）完成一些复杂的工作。 Automator还能横跨很多不同种类的程序，包括： 查找器、Safari网络浏览器、iCal、地址簿或者其他的一些程序。 它还能和一些第三方的程序一起工作，如微软的Office、Adobe公司的Photoshop或者Pixelmator等。
这里我们将使用Automator为xcode定制一个很简单【服务】，来实现一键打包功能。 

* 打开Automator
* 新建一个service 
![image](http://129.204.47.207/img/blog/20161211-2-2.png)

* 修改服务需要的输入、选取需要服务的应用 
![image](http://129.204.47.207/img/blog/20161211-2-3.png)

* 加入流程一 
Automator应用界面左侧，打开资料库面板，点击【变量】，在搜索框中输入“path”，选择目标路径变量，拖拽到中间区域 
![image](http://129.204.47.207/img/blog/20161211-2-4.png)

* 拖拽到中间区域 
![image](http://129.204.47.207/img/blog/20161211-2-5.png)

* 选取项目工程根路径 
在 Automator 应用界面的下方，双击刚才【Destination Path】，在弹出面板中选择工程路径，选择后点击【完成】 
![image](http://129.204.47.207/img/blog/20161211-2-6.png)

* 加入流程二 
automator应用界面左侧，打开资料库面板，点击【操作】，在搜索框中输入“applescript”，选择运行AppleScript，拖拽到中间区域步骤1的下方 
![image](http://129.204.47.207/img/blog/20161211-2-7.png)

* 拖拽到中间区域 
![image](http://129.204.47.207/img/blog/20161211-2-8.png)

* 编辑 Applescript 
在代码框中输入以下内容 

{% codeblock lang:sh %}
    on run {input, parameters}

        tell application "Terminal"
            activate
            do script "cd " & input & " && . BuildScript/xcode-archive-release.sh"
        end tell

        return input
    end run
{% endcodeblock %}


脚本文件 xcode-archive-release.sh 我放到了工程目录的 BuildScript 文件夹里面，脚本打包请参见{% link 自动化测试（脚本打包） https://xudusheng.github.io/2016/12/09/自动化测试（脚本打包）/ %}
这段 Applescript 脚本做的事就是 打开 Terminal，cd 到工程目录，运行BuildScript文件夹里面的脚本文件。

* 最终看起来是这个样子的 
![image](http://129.204.47.207/img/blog/20161211-2-9.png)

*
最后，保存文件，输入文件名字就可以了。打开xcode，看看是否有了新的 service 选项。
服务所在文件：/Users/zhengda/Library/Services文件下，欲修改或者删除相关服务，可以前往该文件夹，找到对应的服务进行对应的操作即可。








































