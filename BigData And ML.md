##### Hadoop安装与命令

###### 常用命令

rz：上传文件到 linux 机器，为避免大文件中断或者出错，使用 rz -be 

单独用rz会有两个问题：上传中断、上传文件变化（md5不同），解决办法是上传是用rz -be，并且去掉弹出的对话框中“Upload files as ASCII”前的勾选

sz：从linux服务器拷贝文件到本机，本机目录。默认下载到的本机目录：Options - Session Options - X/Y/Zmodem



###### Hadoop fs 常用命令

| hadoop fs -mkdir         | 创建HDFS目录                                  |
| ------------------------ | --------------------------------------------- |
| hadoop fs -ls            | 列出HDFS目录                                  |
| hadoop fs -copyFromLocal | 使用-copyFromLocal复制本地文件（local）到HDFS |
| hadoop fs -put           | 使用-put复制本地（local）文件到HDFS           |
| hadoop fs -copyToLocal   | 将HDFS上的文件复制到本地（local）             |
| hadoop fs -get           | 将HDFS上的文件复制到本地（local）             |
| hadoop fs -cp            | 复制HDFS文件                                  |
| hadoop fs -rm            | 删除HDFS文件                                  |
| hadoop fs -cat           | 列出HDFS目录下的文件的内容                    |

