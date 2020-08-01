##### Ionic

###### 1.环境

npm install -g @ionic/cli



###### 2.重新安装依赖

npm install。注意需要管理员权限，windows使用管理员打开控制台



###### 3.平台

使用原生功能之前，先添加平台

npm i -g cordova





##### Android

###### 环境

1.安装 Android Studio，https://developer.android.google.cn/studio/

2.SDK代理，Configure –> settings –> Appearance & Behavior –> System Settings –> HTTP Proxy，选中Auto-detect proxy settings，勾选下方Automatic proxy configuration URL，填入国内的某个镜像站。这里，我选择的是mirrors.neusoft.edu.cn:80

-- 没什么用，代理，直接取消，下一步后，在后面再让其自动下载SDK等

3.环境变量：ANDROID_HOME：SDK目录，添加到Path：%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\tools

ANDROID_SDK_HOME可以不设置

4.Gradle，下载源代码版本 6.5.1（Download: binary-only），直接解压，我们在系统变量中新增一个GRADLE_HOME值为解压后的路径，然后我们还需要修改Path变量，将Gradle的bin目录添加进去，我们在Path变量的最后面添加;%GRADLE_HOME%\bin。gradle -v查看版本。

-- 目前IONIC环境 Gradle版本是 4.10.3，会自动下载



###### 真机调试

npm i -g native-run

ionic cordova run android -l



##### 问题

###### 1.安装SDK包失败

Android Studio：Installation did not complete successful.See the IDE log for details

右键Android Studio，点用管理员身份运行即可



###### 2.nav 导航传参数

```
async sendArg() {
        await this.nav.navigateForward(['/demoArg'], {
            queryParams: {
                userName: this.uName
            }
        });
    }
```

```
ngOnInit() {
        // 需要接收用户传递来的参数,并显示在视图中
        this.activeRoute.queryParams.subscribe((params: Params) => {
            console.log(params);
            this.uName = params.userName;
        })
    }
```

###### 3.NullInjectorError: R3InjectorError

NullInjectorError: R3InjectorError(PrjqueslistPageModule)[ImagePicker -> ImagePicker

需要在module.ts 注册

```
import { ImagePicker } from '@ionic-native/image-picker/ngx';

import { Tab2PageRoutingModule } from './tab2-routing.module';

@NgModule({
  imports: [
    IonicModule,
    CommonModule,
    FormsModule,
    ExploreContainerComponentModule,
    Tab2PageRoutingModule
  ],
  declarations: [Tab2Page],
  providers: [
    ImagePicker
  ]
})
```



###### 4.ImagePicker

ionic cordova plugin add cordova-plugin-telerik-imagepicker --variable PHOTO_LIBRARY_USAGE_DESCRIPTION="your usage message"

如果相册没有照片，显示也是空白，避免被误导认为是插件有问题......



##### 打包

###### 图标与名称

在项目中resources中替换成自己的图标和启动画面即可

在config.xml 修改包名

打正式包和升级打包同原生的类似，在Androidmanifest.xml修改版本号和版本名



##### 操作，IDE

###### 1.vscode 双引号警告移除

打开 tslint.json文件，把 quotemark 里面的ture改成false



