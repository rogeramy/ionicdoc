##### Android

###### 环境

1.安装 Android Studio，https://developer.android.google.cn/studio/

2.SDK代理，Configure –> settings –> Appearance & Behavior –> System Settings –> HTTP Proxy，选中Auto-detect proxy settings，勾选下方Automatic proxy configuration URL，填入国内的某个镜像站。这里，我选择的是mirrors.neusoft.edu.cn:80

-- 没什么用，代理，直接取消，下一步后，在后面再让其自动下载SDK等

3.环境变量：ANDROID_HOME：SDK目录，添加到Path：%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\tools

ANDROID_SDK_HOME可以不设置

4.Gradle，下载源代码版本 6.5.1，直接解压，我们在系统变量中新增一个GRADLE_HOME值为解压后的路径，然后我们还需要修改Path变量，将Gradle的bin目录添加进去，我们在Path变量的最后面添加;%GRADLE_HOME%\bin。gradle -v查看版本。

-- 目前IONIC环境 Gradle版本是 4.10.3，会自动下载



##### 问题

1.安装SDK包失败

Android Studio：Installation did not complete successful.See the IDE log for details

右键Android Studio，点用管理员身份运行即可



##### 打包

###### 图标与名称

在项目中resources中替换成自己的图标和启动画面即可

在config.xml 修改包名

打正式包和升级打包同原生的类似，在Androidmanifest.xml修改版本号和版本名

