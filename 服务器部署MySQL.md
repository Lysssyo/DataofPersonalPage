# 服务器部署MySQL

​		买好服务器后，用服务器连接工具XShell连接服务器。

​		下载宝塔面板：[宝塔面板 - 简单好用的Linux/Windows服务器运维管理面板 (bt.cn)](https://www.bt.cn/new/index.html?invite_code=MV9waHp3Y2w=)

​		用宝塔面板安装MySQL

> 安装MySQL就够了，其他的用的时候可以在软件商店安装。

​		用的时候可以在XShell连接mySQL新建数据库，“宝塔面板”--“数据库”点击“从服务器获取”，然后就能获取到新建的数据库，根据宝塔面板的数据库名，用户名以及密码可以在Idea或者Navicat远程连接（用不了root登录）

> 例如：
>
> XShell中输入：
>
> `mysql -u root -p`
>
> 输入密码，进入msql
>
> 然后：
>
> `create database DBforComDesignMatch`，创建一个新的数据库（`DBforComDesignMatch`为数据库名）
>
> 再然后，宝塔面板点数据库----从服务器获取就能获取到刚新建的数据库，根据给出的数据库名，用户名和密码就可以远程连接。
>
> ![image-20240312144743511](BuildALIYUN.assets/image-20240312144743511.png)
>
> ![image-20240312145135751](BuildALIYUN.assets/image-20240312145135751.png)
>
> 但是，不能用root连接：
>
> ![image-20240312145234264](BuildALIYUN.assets/image-20240312145234264.png)