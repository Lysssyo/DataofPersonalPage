# Springboot项目上传服务器

## 1.运行java程序

​		项目完成后，在Maven先clean后package，会在项目的根目录下生成target文件夹，target中的jar包就是要上传到服务器的文件。

![image-20240226134813144](SpringbootUpload.assets/image-20240226134813144.png)					![image-20240226134945338](SpringbootUpload.assets/image-20240226134945338.png)

​		打开服务器连接工具（以XShell、Xftp为例），连接服务器，点击“新建文件传输”

![image-20240226135626454](SpringbootUpload.assets/image-20240226135626454.png)

​		把打好的jar包拖进去

![image-20240226135748532](SpringbootUpload.assets/image-20240226135748532.png)

​		然后在终端中输入命令：

```
java -jar 你的jar包名字.jar
```

![image-20240226141211094](SpringbootUpload.assets/image-20240226141211094.png)

​		之后java程序就会开始运行，但是此时java程序是在前台运行，关闭终端java程序也会被关闭，因此需要再输入一条命令，使java程序可以在后台运行：

```
nohup java -jar 你的jar包名字.jar > log.file 2>&1 &
```

![image-20240226141325474](SpringbootUpload.assets/image-20240226141325474.png)

## 2.结束进程

查看所有端口的进程：

```
netstat -tuln
```

​		![image-20240226140539101](SpringbootUpload.assets/image-20240226140539101.png)

或者：

```
sudo lsof -i -P -n | grep LISTEN
```

![image-20240226140937159](SpringbootUpload.assets/image-20240226140937159.png)

根据进程ID结束进程（以上图443端口的java程序为例）

```
sudo kill -9 33845
```

