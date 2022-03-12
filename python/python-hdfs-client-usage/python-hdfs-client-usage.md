# Python HDFS客户端使用

## 前置说明

我使用的是Anaconda的程序进行Python环境的管理的。

## 安装

```shell
pip install hdfs==2.3.0
```

这里有几点需要注意的

1. 如果使用的是Anaconda创建的虚拟环境的话，一定要注意pip命令的话，要使用虚拟环境下的pip命令，不然很容易产生模块找不到的错误的
2. 我的命令行里面选用的是hdfs的版本的话，是根据我所使用的Hadoop HDFS的版本决定的，并不是越新越好的，比如我现在环境使用的是Hadoop HDFS 3.2.0 我查看到这个版本是2019年1月21号的时候发版的，所以在选择Python HDFS的包的时候，选择比它晚一些时候发版的包就可以了，这样可以尽量避免（不能百分百保证）包版本的差异导致的一些不兼容的问题，

![image-20220312090513816](/Users/jackpan/JackPanDocuments/jack-doc/jack-honor/python/python-hdfs-client-usage/image-20220312090513816.png)

![image-20220312091625649](/Users/jackpan/JackPanDocuments/jack-doc/jack-honor/python/python-hdfs-client-usage/image-20220312091625649.png)

这样我们的基础环境就准备完成了

## 客户端代码案例

```python
from hdfs import InsecureClient

client = InsecureClient('http://192.168.101.151:9870', user='lczydw')
rootPath = '/OCR_FILE_TEST'
# 可以打印这个文件目录的文件列表
fnames = client.list(rootPath)
for fileName in fnames:
    print(fileName)
    
# 下载HDFS文件到本地电脑中
client.download(path, localRootPath, n_threads=2)
```

以上就是HDFS简单的一个使用方法的入门，更多关于API的详细使用的话，可以参考官方网站的文档说明

官方网站的文档 https://hdfscli.readthedocs.io/en/latest/quickstart.html

API文档： https://hdfscli.readthedocs.io/en/latest/api.html#api-reference

