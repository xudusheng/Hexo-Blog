---
title: 自动化测试（Test Flight）
date: 2017-03-02
categories: [自动化测试]
---

应用的beta版测试一直让iOS开发者非常头疼，所以苹果在`WWDC 2014`上高调地宣布将`TestFlight`（官网：http://testflightapp.com/ )加入iOS 8，并在原先的基础上进行了更多完善和提升。

以往为测试者的大致流程是：
1、测试者把设备的`UDID`发给开发者；
2、开发者登录苹果的`Developer Portal`；
3、开发者将测试者的设备添加到`Devices`；
4、开发者将新设备添加至适当的`Provisioning Profiles`中；
5、开发者使用`Profile`更新应用；
6、最后开发者将应用分发给测试者。
当然了，`Xcode10`以后，以上步骤在`Xcode`中通过`Automatically manager signing`自动完成。
这里还有个长期让开发者诟病的限制，那就是一个开发者账号只能添加 100 个设备。

为了避开以上的限制，部分开发者也可能选择通过Ad Hoc发布的形式进行测试包的分发。

传统测试分发的缺点：
    1、测试包的分发步骤繁琐：
    添加设备->打ipa包->上传ipa包->生成二维码以供下载
    通过Ad Hoc分发步骤：打ipa包->上传ipa包->生成二维码以供下载
    最后：设备连接开发机，通过Xcode一个一个打包，这个有点烦了。
    有人说搭建持续化集成环境，实现一键打包。这个也是个解决办法，有兴趣的朋友可以参考`自动化测试（Jenkins+GitHub+fir-cli自动打包发布）`一文，这里我们不做讨论。

2、ipa包的托管问题突出：
    将ipa包托管到第三方，不安全；
    将ipa包放在自己的服务器上，又需要后台另外开发一套这样的服务，开发成本高；
3、成本问题：
    通过Ad Hoc分发的需要额外申请$299的企业级账号，不便宜呢。
4、由于开发者的疏忽，可能将测试包当成生产包提交到AppStore，这个就严重了。

TestFlight介绍
苹果在`WWDC 2014`上高调地宣布将iOS开发人员可通过`TestFlight`进行测试，并将其整合到了`iTunes Connect`中。`TestFlight`随`iOS8`一起发布，因此开发者没法在旧的iOS版本或者`Android`版本上使用 `TestFlight`。`TestFlight`将可以让用户浏览程序描述以及测试备注。测试备注将会让开发者知道哪里需要提高、哪里需要改进。同时测试者也可以通过 TestFlight 软件向开发者发邮件提供他们的反馈。`TestFlight`同时还提供崩溃日志的查看，当设备上的软件崩溃时，会产生一条错误日志。

`TestFlight`的测试方式分为两种，一种是内部测试，一种是外部测试。
    外部测试人员的上限是`2000`人，发布测试包之前，需要先通过苹果的审核，一般审核会在一天左右。
    内部测试最多可以邀请`25`个内部成员，App上传到`iTunes Connect`上之后内部成员就可以开始进行内部测试了，无需审核。
    当应用程序的新版本被上传或者通过审核后，它将有`30天`的有效期。如果30天之后开发者没有发布新的版本，测试者将无法运行设备上的测试版应用程序，直到开发者发布新版本，测试者才可以再度运行程序。除了要上传二进制数据外，开发者还必须提交程序的元数据，包括应用程序的描述以及测试者需要测试的信息。
    注：我们公司目前有 10 个测试设备，所有的设备使用同一个Apple ID登录，进行`TestFlight`内部测试。

内部测试
1.添加测试者的Apple ID，如果该Apple ID未开通iTunes Connect，则向该测试者发送开通iTunes Connect邮件；
2.测试者登录并激活iTunes Connect；
3.开发中添加内侧账户，发送邮件邀请测试者；
4.测试者接受邀请；
5.测试者通过TestFlight应用程序安装app。









































