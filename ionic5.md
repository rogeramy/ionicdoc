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



###### 5.WARNING: sanitizing unsafe URL value

在项目中有时候需要在叫页面上显示图片，音频的信息，在项目中我把图片和音频等文件都转成了base64格式上传到服务器数据库。

在Ionic2项目中比如使用：img，iframe，audio标签的src，a标签的href，需要使用数据库传来的base64数据时，变量直接赋值到url绑定数据的话，会报错，同样，需要引入外部url的资源链接也会报错

网上找了很多资料：才发现是Ionic2和TypeScript中对外部url资源链接做了安全限制
官网文档中对此做了解释：[](http://http://g.co/ng/security#xss)

```
//1.在需要使用外部url链接的ts文件中，引入DomSanitizer类
import { DomSanitizer } from '@angular/platform-browser';  

export class safeHtml{  

  safeUrl : any;  

  constructor(private sanitizer: DomSanitizer) {}  

  //2.在需要使用转换后的url地方加上
  getSafeUrl(url){
      this.safeUrl = this.sanitizer.bypassSecurityTrustResourceUrl(url); 
  }
```

```
<audio controls="controls" [src]="safeUrl"></audio>
```



###### 6.图片预览

在手机上预览图片，预览缩略图，使用 base64方式，file url 方式有权限限制



###### 7.Unexpected token h in JSON at position 0 at JSON.parse | http.get 返回格式设置

通过 http.get 返回的数据，默认是 json格式，会进行json转换，如果返回的是 url 链接，可能导致转换出错。修改http.get 返回格式为text

如果显示设置了 responseType，必须：'text' as 'text'，'json' as 'json'，否则默认是 json

```
 const token = this.userdata.getUserToken();

    const headers = {
      headers: new HttpHeaders().set("Authorization", "Bearer " + token), responseType: 'text' as 'text'
    };

    return this.http.get(this.getUrl() + urlStrAndParam, headers);
```

[详细bug说明](https://www.cnblogs.com/wangtingnoblog/p/10322483.html)



###### 8.ion-select默认绑定选项

```
 <ion-select interface="popover" value="{{obj.quesStatusId}}" (ionChange)="quesStatusChange($event)">
        <ion-select-option value=1>进行中</ion-select-option>
        <ion-select-option value=100>完成</ion-select-option>
      </ion-select>
```



###### 9.安卓不能访问http地址

在ionic4打包成Android包以后真机运行的时候可能会报ERR_ClEARTEXT_NOT_PERMITTED错误，原因是Android8以后对网络安全进行了限制，不允许明文传输，即http请求

1、Android版本降级，删除原Android8以上版本，添加Android7以下版本

```

ionic cordova platform rm android//执行删除Android平台命令
 
ionic cordova plarform add android@7.X//添加Android7版本或者以下版本
```

2、放开对HTTP的限制

需要对下图2个文件进行修改，文件路径在：项目名/platforms/android/app/src/main/res/xml/network_security_config.xml和项目名/platforms/android/app/src/main/AndroidManifest.xml

首先修改network_security_config.xml文件,localhost修改为需要访问的第三方接口地址

修改AndroidManifest.xml文件，在<application />标签添加android:networkSecurityConfig="@xml/network_security_config" android:usesCleartextTraffic="true"

```
<application android:networkSecurityConfig="@xml/network_security_config"  android:usesCleartextTraffic="true"></application>
```



##### 打包

###### 图标与名称

在项目中resources中替换成自己的图标和启动画面即可

在config.xml 修改包名

打正式包和升级打包同原生的类似，在Androidmanifest.xml修改版本号和版本名



##### 操作，IDE

###### 1.vscode 双引号警告移除

打开 tslint.json文件，把 quotemark 里面的ture改成false



