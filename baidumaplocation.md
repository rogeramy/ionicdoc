https://github.com/aruis/cordova-plugin-baidumaplocation

申请key
1.更改config.xml文件包名称
<widget id="io.ionic.mipsapp" version="0.0.1" xmlns="http://www.w3.org/ns/widgets" xmlns:cdv="http://cordova.apache.org/ns/1.0">
    <name>mipsapp</name>
    
安装

cordova plugin add cordova-plugin-baidumaplocation --variable ANDROID_KEY="<API_KEY_ANDROID>" --variable IOS_KEY="<API_KEY_IOS>"
# 此处的API_KEY_XX来自于第一步，直接替换<API_KEY_XX>，也可以最后跟 --save 参数，将插件信息保存到config.xml中
# 如果只需要Android端或者IOS端，可以只填写一个相应的AK，但是都不填肯定不行


必须安装 @3.2.0 版本，最新版本可以获取对象，方法无法返回数据
ionic cordova platform rm android 
ionic cordova plugin rm cordova-plugin-baidumaplocation 
ionic cordova plugin add cordova-plugin-baidumaplocation@3.2.0 -–variable ANDROID_KEY=”12321121” -–variable IOS_KEY=value

核心代码

在顶端声明：declare const baidumap_location: any; 
if (typeof baidumap_location === “undefined”) { 
alert(“baidumap_location is undefined”); 
return; 
}; 
baidumap_location.getCurrentPosition(function (result) { 
alert(JSON.stringify(result, null, 4)); 
}, function (error) { 
alert(error); 
});



https://blog.csdn.net/cherryshi520/article/details/80737203
