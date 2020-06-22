### 简介

FastDFS是用c语言编写的一款开源的分布式文件系统。FastDFS为互联网量身定制，充分考虑了冗余备份、负载均衡、线性扩容等机制，并注重高可用、高性能等指标，使用FastDFS很容易搭建一套高性能的文件服务器集群提供文件上传、下载等服务

[介绍](https://blog.csdn.net/taojin12/article/details/88380375)



### 与 HDFS比较



### DocSys Web的文件管理系统

码云： https://gitee.com/RainyGao/DocSys

GitHub: https://github.com/RainyGao-GitHub/DocSys   



### Minio

[guide](https://docs.minio.io/docs/)

[demo1](https://zhuanlan.zhihu.com/p/142267115?from_voters_page=true)

[demo2](https://github.com/yzcheng90/X-SpringBoot)

[demo3](https://blog.csdn.net/qq_15273441/article/details/100094667)

##### 官方地址

[E 官网](https://min.io/)

[C 文档，比E官网内容少](https://docs.min.io/cn/)

[github](https://github.com/minio)

[github windows 下作为服务部署](https://github.com/minio/minio-service/tree/master/windows)



##### 安装

###### Windows

1.直接控制台启动，Server 系统使用管理员运行 cmd

2.安装成windows 服务，从服务启动，这个方式更优。从 [github地址](https://github.com/minio/minio-service/tree/master/windows) 下载 ps1文件（windows powershell 文件），和minio.exe放在同一个文件夹下，用记事本打开修改其中的参数，比如用户名密码，启动参数，启动参数和控制台启动方式一样，可以设置文件存储目录。设置完成后点击运行ps1文件，在windows服务内可以看到已经生成了名为MinIO的服务，同时对应生成xml配置文件，可以修改配置文件内的用户名密码等参数。这种方式需要连接互联网下载服务安装包，如果不能联网，可以参考github内手动安装方式

##### 名字规范

文件名称：到秒的时间字符串（时间24小时制）+文件名



##### Java API 操作

###### 上传

```
@RequestMapping(value = "/uploadFile", method = RequestMethod.POST)
    public ReturnValue uploadFile(@RequestParam("file") MultipartFile file) throws IOException {}
```

1.RequestParam(”file") 来自客户端上传空间的名称 "file"，必须一致

2.名称：格式化时间，getOriginalFilename 才是获取带后缀的文件名，保存到MinIO的文件名必须包含后缀，便于文件的下载、打开与分享浏览

```
DateFormat df = new SimpleDateFormat("yyyyMMddHHmmss");
String fileName =df.format(new Date()) +"_"+originalFilename;
```

3.上传文件的大小与MIME 类型，为了可以在浏览器浏览，需要设置正确的MIME类型，比如pdf和图片，iptions的文件长度必须和上传的文件长度一致

```
PutObjectOptions options = new PutObjectOptions(file.getSize(), -1);
options.setContentType(file.getContentType());
```

4.返回桶名和文件名，作为唯一标识



###### Postman 测试

1.请求类型：Post

2.Content-Type:multipart/form-data

3.body：选择“form-data”，key填写”file“或者其它与服务端接受参数一致的名称，key的类型选择”file"，就可以在value处选择文件测试了