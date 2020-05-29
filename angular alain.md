#### ng-alain环境安装

node:v12.16.1
npm:6.13.4

设置正确淘宝镜像，能访问外网的飘过，注意必须设置 sass
npm config set registry https://registry.npm.taobao.org
npm config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass

恢复
npm config delete registry
npm config delete sass_binary_site

安装yarn
1.22.1
npm install -g yarn

设置正确yarn淘宝镜像，必须设置sass
yarn config set registry https://registry.npm.taobao.org
yarn config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass

恢复
yarn config delete registry
yarn config delete sass_binary_site

安装 angular/cli

angular/cli:9.0.5

从github拷贝脚手架


yarn

npm start


在 ng-alain 中，一个完整的 Angular 应用从前端 UI 交互到服务端处理流程是这样的：
1、首次启动 Angular 执行 APP_INITIALIZER；
2、UI 组件交互操作；
3、使用 HttpClient 发送请求；
4、触发用户认证拦截器 @delon/auth，统一加入 token 参数；
  a、若未存在 `token` 或已过期中断后续请求，直接跳转至登录页；

5、触发默认拦截器，统一处理前缀等信息；
6、获取服务端返回；
7、触发默认拦截器，统一处理请求异常、业务异常等；
8、数据更新，并刷新 UI。


ng-alain 默认装载了两个拦截器：@delon/auth 用户认证和默认拦截器

本身是为 ng-alain 脚手架提供的一个用户认证模块，包含主流的 JWT（Json Web Token）和一个相对通用 Simple Web Token，而其核心是对认证过程进一步处理。而通常其核心在于用户 Token 的获取、使用环节。
同时，@delon/auth 并不会关心用户界面是怎么样，只需要当登录成功后将后端返回的数据交给 ITokenService，它会帮你存储在 localStorage（默认） 当中；当发起一个网络请求时，它会在自动在 header（默认） 当中加入相应的 token 信息。

因此，@delon/auth 不限于 ng-alain 脚手架，任何 Angular 项目都可以使用它。

默认装载了 SimpleInterceptor 拦截器，意味者一开始使用 ng-alain 为什么会无缘无故无法正确请求，而是直接抛出异常。

服务端如果是基于ASP.NET API 验证方式，传入token为：Authorization: bearer，需要重写
a) ng-alain的用户认证默认发给后端的认信息的key值是token，如过要更改可以在src -> app -> delon.module.ts中修改如下
import { DelonAuthConfig } from '@delon/auth';
export function fnDelonAuthConfig(): DelonAuthConfig {
  // return {
  //   ...new DelonAuthConfig(),
  //   login_url: '/passport/login',
  // };
  return Object.assign(new DelonAuthConfig(), <DelonAuthConfig>{
    login_url: '/passport/login',
    token_send_key: 'Authorization',
    token_send_template: 'Bearer ${token}'
  });

}

设置后台接口服务的basicUrl

修改 src -> environments 文件夹下
生产环境和开发环境的ts文件中的 SERVER_URL
export const environment = {
  SERVER_URL: `http://服务器地址`,
  ....
};

** alain 使用 http post 提示错误：The request entity's media type 'text/plain' is not supported for this resource
.. 客户端不需要使用 json 工具序列化成字符串，再post，直接 post 内传对象 .post('.../api/QLogic/AddQues', postdata, headers)，不能使用 JSON.stringify，否则request头就不是Content-Type: application/json，而是Content-Type: text/plain

** alain 发布，IIS网站的子目录下，需要重写 build ，否则导致引用的css，script文件路径不正确
.. package.json 文件，"build": "npm run color-less && ng build --prod --build-optimizer --base-href ./", 加上后面这部分： --base-href ./

** npm start 编译时候出错，JavaScript heap out of memory
.. 初步判断是执行 color-less文件内容时候出错，可能和agalain升级到支持angular8有关，暂时解决办法先把执行color-less代码去掉：package.json文件内改成：
   "start": "ng serve -o","build": "ng build --prod --build-optimizer --base-href ./",

** <nz-option [nzValue]="1" [nzLabel]="'南向'"></nz-option> nzValue在中括号内，值1是数字，<nz-option nzValue="1" [nzLabel]="'南向'"></nz-option> 这种方式1是字符串，如果后台ngmodel数据是数字，则无法选中

** select 重置选择（去掉选择项），设置ngModel对应的值为null，不能设置-1等其它数字



#### ngx-echarts 使用

##### 安装

[https://www.npmjs.com/package/ngx-echarts](ngx-echarts官网)

使用 yarn 安装，安装前先设置 yarn 的淘宝镜像

设置正确yarn淘宝镜像，必须设置sass
yarn config set registry https://registry.npm.taobao.org
yarn config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass

恢复：
yarn config delete registry
yarn config delete sass_binary_site



yarn 安装，转到项目文件目录下：

```
yarn add echarts
yarn add ngx-echarts
yarn add @types/echarts -D
```



##### 问题 Can't bind to 'options' since it isn't a known property

import { NgxEchartsModule } from 'ngx-echarts' 这个不一定放在app.module.ts中，

就以直接下载的ng-alain demo为例，如果想在http://localhost:4200/#/dashboard 页面中使用 echart，则需要将 import { NgxEchartsModule } from 'ngx-echarts' 放在 routes.module.ts文件中 ，代码如下所示

import{NgxEchartsModule}from 'ngx-echarts';而在alain框架中，页面都是在route模块下的二级路由中，在appmodule中引入在route模块下的页面并获取不到，因此要把ngx-echarts模块在route.module.ts中引入



##### echarts tree 点击收起同级兄弟节点

在树中一个节点的展示与收缩是通过节点的collapsed属性进行控制的，collapsed = true，收缩；false，节点展开。解决思路：首先绑定一个点击事件，我们就可以知道被点击的节点的params.name,.既然我们知道根节点，我们就可以对根节点进行往下遍历（DFS），得知每一个节点的深度，深度与点击节点相同的即是同级节点。最终将所有节点对应的collapsed赋对应的值就好了。

可以使用name作为唯一标记，也可以定义其他属性，比如id。

更简单的方式，假如需求只需要收起倒数第二排节点，那么可以给倒数第二排节点给予一个特殊标记（比如 needC:true，点击时候遍历到存在该特殊标记的全部收起，如果是当前点击的节点，则忽略。

```
let current_dep = 0;
    myChart.on('click', function(params) {
        dfs(option.series[0].data, params.name, 0);
        solve(option.series[0].data, params.name, current_dep);
        myChart.clear();
        myChart.setOption(option);
    });
    function dfs(array, id, dep) {
        array.forEach((obj, i) => {
            obj.dep = dep;
            if(obj.name === id) {
                current_dep = dep;
                console.log("current: " + current_dep);
            } else if(obj.children !== undefined && obj.children.length > 0) {
                return dfs(obj.children, id, dep + 1);
            }
        })
    }
    function solve(array, id, current_dep) {
        array.forEach((obj, i) => {
            if(obj.dep < current_dep) {
                obj.collapsed = false;
                if(obj.children && obj.children.length > 0) {
                    solve(obj.children, id, current_dep);
                }
            } else if(obj.dep === current_dep) {
                if(obj.name === id) {
                    obj.collapsed = false;
                } else {
                    obj.collapsed = true;
                }
            }
        })
    }
```



##### 4.echarts 钻取

[地图](https://www.cnblogs.com/timeddd/p/11169188.html)

https://github.com/triplestudio/helloworld/releases

[Bar](https://blog.csdn.net/lifhcode/article/details/81221693)



##### 5.echarts 动态更新数据

```
<div echarts [options]="chartOption"  (chartInit)="onChartInit($event)"></div>
```

在 onChartInit中获取 echarts对象

```
 this.chartOption = option;
    this.chartOption.xAxis[0].data = timeSeries;
    this.chartOption.series = item;
    if (this.echartsIntance) {
      this.echartsIntance.clear();
      this.echartsIntance.setOption(this.chartOption, true);
    }
```

注意，option 不要设置 echarts 对象类型，或者设置到具体的图表对象，否则可能出现   this.chartOption.xAxis[0].data 找不到 data 对象



#### 问题记录

##### 1.alain封装G2组件不显示图表

图表不能显示，拖动浏览器可以显示，主要因为图表在数据没有准备好之前先渲染了。需要配置一个延迟加载量，但延迟加载时间是无法把握的，如果是来自网络的数据源，最佳做法是使用 *ngIf 做判断，数据源准备好之后才渲染



##### 2.npm build maximum exceeded for initial

打开**angular.json**文件，找到budgets看到这段：

"budgets": [
   {
      "type": "initial",
      "maximumWarning": "6mb",
      "maximumError": "6mb"
   }
]

修改为更大值



##### 3.发布到IIS，访问时候css等文件404错误

*alain 发布，IIS网站的子目录下，需要重写 build ，否则导致引用的css，script文件路径不正确
.. package.json 文件，"build": "npm run color-less && ng build --prod --build-optimizer --base-href ./", 加上后面这部分： --base-href ./



##### 4.ng-alain无法跳转到登录页面

在修改从服务端获取app导航数据后，无法显示登录页面，拦截器类里面去掉http异常的向上抛出，否则不能跳转到登录页面

```
// 必须注释，不然跳转不到登录页面
    // if (ev instanceof HttpErrorResponse) {
    //   return throwError(ev);
    // } else {
    //   return of(ev);
    // }
    return of(ev);
```



##### 5.更改登录页面背景图片

C:\Users\rogerwin2020\Desktop\ng-alain-master\src\app\layout\passport\passport.component.less



##### 6.xlsx 导出提示 XLSX没有定义

没有引用到对应的 xlsx.full.min.js 文件，默认使用：//cdn.bootcss.com/xlsx/0.15.6/xlsx.full.min.js，如果网络限制，则存在错误信息，可以更改为其它url或者使用本地文件，https://oss.sheetjs.com/sheetjs/xlsx.full.min.js

8.X 版本修改，app.module.ts文件

```
import { XlsxConfig } from '@delon/abc';
export function fnXlsxConfig(): XlsxConfig {
  return { ...new XlsxConfig(), ...{ url: 'https://oss.sheetjs.com/sheetjs/xlsx.full.min.js' } as XlsxConfig };
}
```

```
providers: [...LANG_PROVIDES, ...INTERCEPTOR_PROVIDES, ...I18NSERVICE_PROVIDES, ...APPINIT_PROVIDES, ApiHelp, { provide: XlsxConfig, useFactory: fnXlsxConfig }],
```



9.X版本参考官方文档

也可以在使用的页面，引入xlsxconfig，init()内指定url，this.xlsxconfig.url =''，不建议