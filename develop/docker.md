****
*docker入门后进行的深化*

# docker数据卷
****

进行容器的持久化和同步操作，容器间也可以数据共享

## 使用
```
docker run -it -v 主句目录：容器目录 [image] /bin/bash
#要使用据对路径
//之后用命令查看容器内信息
docker inspect CONTAINER
//检查返回的mount信息查看有没有挂载成功
``` 
ps：主机目录会直接覆盖掉容器目录

这样运行的容器只需要在主机进行文件修改，挂载的目录会自动同步


## docker mysql例子

### 挂载
```
docker run -d -p 3306:3306 -v /home/mysql/conf:/etc/mysql/conf.d 
							-v /home/mysql/data:/var/lib/mysql
							-e MYSQL_ROOT_PASSWORD=123456
							--name mysql0
							mysql
```
容器被删后挂载到本低的数据卷依旧没有丢失

## 具名和匿名挂载
### 匿名卷挂载
```
docker run -d -P --name nginx0 -v /etc/nginx nginx
#可以通过docker volume ls查看卷信息
#-v的时候只写了容器内路径没有写主机路径
```

### 具名卷挂载（一般使用这种）
```
docker run -d -P --name nginx0 -v some-nginx:/etc/nginx nginx
#-v的时候写了卷名和容器路径，注意卷名和主机路径的区别
```

### 拓展
```
#权限设置
ro:只读权限
rw:可读可写
docker run -d -P --name nginx0 -v some-nginx:/etc/nginx:ro nginx
docker run -d -P --name nginx0 -v some-nginx:/etc/nginx:rw nginx

#只要看到ro就说明这个路径只能通过宿主机进行修改，容器内无法操作
```



# dockerfile镜像构建文件
****
*是一堆命令脚本*

```
#这是一个基础的dockerfile进行卷挂载的操作

FROM ctenos
VOLUME ["volume01","volunme02"]
CMD echo "----END---"
CMD /bin/bash
```

```
#这是根据dockerfile文件创建镜像的命令
docker build -f [dockerfile文件路径] -t [tag]
```

### 数据卷容器

*实现不同服务器之间的数据挂载继承*
- 1.首先我们通过上述dockerfile构建了父容器
- 2.可以通过`docker run -volume-from [容器名称]`

更清楚的说法是多个容器共享相同的挂载操作

#### 数据卷容器挂载例子
```
#多个mysql的数据共享 通过-volume-from参数实现
docker run -d -p 3306:3306 -v /home/mysql/conf:/etc/mysql/conf.d 
							-v /home/mysql/data:/var/lib/mysql
							-e MYSQL_ROOT_PASSWORD=123456
							--name mysql0
							mysql

docker run docker run -d -p 3306:3306 
							-volume-from mysql0
							-e MYSQL_ROOT_PASSWORD=123456
							--name mysql0
							mysql
```


**总结**：数据本地化通过-v参数，有两种方式，具名挂载和匿名挂载。此外还有一种方式是通过dockerfile构建镜像挂载，像上述例子中的那样，其它镜像通过-volume-from参数进行指定经常哪个镜像的挂载。


### dockerfile的构建过程

**基础知识**：
- 1、每个保留关键字（指令）都是大写字母
- 2、执行从上到下执行
- 3、#表示注释
- 4、每个指令都会创建提交一个新的镜像层，并提交

dockerfile是面向开发的dockerfile逐渐成为企业交复的标准

#### dockerfile的指令
```
FROM                                #基础镜像
MAINTAINER                          #镜像是谁写的：姓名+邮箱
RUN                                 #镜像构建的时候需要运行的命令
ADD                                 #添加创业资金
WORKDIR                             #镜像的工作目录
VOLUME                              #挂载的目录
EXPOST                              #暴露端口
CMD                                 #指定容器启动时候运行的命令，只有最后一个会生效
ENTRYPOINT                          #指定容器启动的时候要运行的命令，可以追加命令
还有一些用的时候查一下
```

### 自己写一个dockerfile
*向基本的centos镜像中下载vim和net-tools从而形成新的镜像*

```
FROM centos
MAINTAINER ATUN<3101625580@qq.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "---END---"
CMD /bin/bash
```

