##### Android

###### 环境

1.安装 Android Studio，https://developer.android.google.cn/studio/

2.SDK代理，Configure –> settings –> Appearance & Behavior –> System Settings –> HTTP Proxy，选中Auto-detect proxy settings，勾选下方Automatic proxy configuration URL，填入国内的某个镜像站。这里，我选择的是mirrors.neusoft.edu.cn:80

-- 没什么用，代理，直接取消，下一步后，在后面再让其自动下载SDK等

3.环境变量：ANDROID_HOME：SDK目录，添加到Path：%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\tools

ANDROID_SDK_HOME

4.Gradle，下载源代码版本 6.5.1，直接解压，我们在系统变量中新增一个GRADLE_HOME值为解压后的路径，然后我们还需要修改Path变量，将Gradle的bin目录添加进去，我们在Path变量的最后面添加;%GRADLE_HOME%\bin。gradle -v查看版本