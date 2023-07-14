---
title: docker的基本教程
hideSummary: True
hideMeta: True
---

参考链接：https://zhuanlan.zhihu.com/p/143156163

# 1. 安装docker
```bash
# 首先安装 Docker
yum -y install docker
dok 
# 然后启动 Docker 服务
service docker start

# 测试安装是否成功
docker -v

# 配置阿里云镜像加速
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://******.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

# 2. 拉取centos镜像
```bash
docker pull centos:centos7
```


# 3. 启动容器
```bash
docker run -itd --name=x1 centos:centos7 /bin/bash 
docker rename 原容器名称 新容器名称
```

# 4. 本项目用到的dockerfile
<!-- RUN cd /usr/src/nginx-1.15.8 \
    && mkdir /usr/local/nginx \
    && ./configure --prefix=/usr/local/nginx && make && make install \
    && ln -s /usr/local/nginx/sbin/nginx /usr/local/sbin/ \
    && nginx
RUN rm -rf /usr/src/nginx-nginx-1.15.8 -->
```
FROM  docker.io/library/centos:centos7 

MAINTAINER xyq xyq@qq.com

RUN yum install wget -y
RUN wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py39_23.5.0-3-Linux-x86_64.sh --no-check-certificate
RUN bash Miniconda3-py39_23.5.0-3-Linux-x86_64.sh -b
RUN /root/miniconda3/bin/conda init
RUN source ~/.bashrc
RUN /root/miniconda3/bin/conda config --add channels defaults
RUN /root/miniconda3/bin/conda config --set show_channel_urls True
RUN /root/miniconda3/bin/conda config --add default_channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
RUN /root/miniconda3/bin/conda config --add default_channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
RUN /root/miniconda3/bin/conda config --add default_channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
RUN /root/miniconda3/bin/conda config --add default_channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
RUN echo -e "custom_channels:\n  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud\n  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud\n  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud\n  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud\n  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud\n  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud\n  deepmodeling: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/" >> ~/.condarc
RUN /root/miniconda3/bin/conda install -c conda-forge kafka-python xgboost pandas -y 
RUN /root/miniconda3/bin/pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
RUN /root/miniconda3/bin/pip install redis==4.6.0 lz4

COPY /app .

```

# 5. 构建镜像

```bash
docker build -t xyq:centos7 .
```


# 6. 暂时退出容器和宿主机交换文件
```bash
docker run -it -v /home/haha/下载:/root microsoft/dotnet:latest /bin/bash
```

# 7. 暂时退出容器
```bash
# Ctrl+P+Q
exit # 前提是启动时加了-d参数让其后台运行
```

# 8. 再次进入容器
```bash
docker exec -it db3 /bin/bash # 或者
```

# 9. 关闭容器
```bash
exit
# Ctrl+D
# Ctrl+P,Q
docker stop 容器名/id
```

# 10. 导出容器成镜像
```bash
docker commit -m "改动信息" -a "作者信息" [container_id] [new_image:tag]
```


# 11. 镜像保存
```bash
docker save -o 将要保存到本地的文件名 要保存的镜像
```

# 12. 上传镜像
```bash
docker login -u cn-south-1@E23KOIFVFVAJD -p f371b92a43cb5048f21bc9e8d06b1fa767 swr.cn-south-1.myhuaweicloud.com

sudo docker tag [{镜像名称}:{版本名称}] swr.cn-south-1.myhuaweicloud.com/{组织名称}/{镜像名称}:{版本名称}
sudo docker push swr.cn-south-1.myhuaweicloud.com/{组织名称}/{镜像名称}:{版本名称}

sudo docker tag xyq:k8s_demo1 swr.cn-south-1.myhuaweicloud.com/bigdata/xyq:k8s_demo1
sudo docker push swr.cn-south-1.myhuaweicloud.com/bigdata/xyq:k8s_demo1
```



# 13. 其他可能用得到的命令

## 13.1. 容器的基本操作
```bash
#查看正在运行容器
docker ps
 
#查看所有容器
docker ps -a
 
#查看最新创建的容器
docker ps -l


# 创建容器
docker create 镜像
	# --创建一个 nginx 容器：docker create nginx
	# --指定容器的name为nginx：docker create --name=nginx nginx
# 创建并启动一个容器
docker run 镜像	--docker run --name nginx1 -d -p 8080:80 nginx
# --name 含义和上文一样，表示创建的容器的名字，-d 表示容器在后台运行，-p 表示将容器的 80 端口映射到宿主机的 8080 端口
# 临时退出容器可以使用会计见Ctrl+P+Q


# 进入一个容器
    # 1:使用docker attach命令
    # 我们使用
docker attach db3 或者 docker attach d48b21a7e439
    # db3是后台容器的NAMES,d48b21a7e439是容器的进程ID  CONTAINER ID
    # 然后就进去了这个容器的ssh界面。
    # 但是它有一个缺点，只要这个连接终止，或者使用了exit命令，容器就会退出后台运行
    # 2:使用docker exec命令
    # 这个命令使用exit命令后，不会退出后台，一般使用这个命令，使用方法如下
docker exec -it db3 /bin/sh # 或者
docker exec -it d48b21a7e439 /bin/bash


#启动容器
docker start id|name
#重启容器
docker restart id|name
#关闭容器
docker stop id|name


#单个删除
docker rm id|name
#批量删除
docker rm $(docker ps -a -q)


#容器详情
docker inspect id|name
 
#查看容器进程
docker top 
 
#查看你容器日记
docker logs id|name
		# -f ：实时日记
		# -t : 显示时间
		# -tail： 控制输出行数

#导出一个已经创建的容器导到一个文件
docker export -o 文件名.tar 容器id
#将文件导入为镜像
docker import 文件名.tar 镜像名:镜像标签
```

## 13.2. 镜像的基本操作


```bash
# 查看所有镜像
docker images
# 搜索镜像
docker search name
# 下载镜像
docker pull name:版本
# 版本可以在Docker hub上查看 

# 删除镜像
docker rmi id|name
```
## 13.3. Dockerfile的编写

```bash
FROM 		#基础镜像，一切从这里开始构建
MAINTAINER	#镜像是谁写的，姓名+邮箱
RUN			#镜像构建时需要运行的命令
ADD			#步骤，tomcat镜像，这个tomcat压缩包；添加内容
WORKDIR		#镜像工作目录
VOLUME		#挂载的目录
EXPOSE		#暴露端口配置
CMD			#指定这个容器启动的时候要运行的命令,只有最后一个会生效，可被替代
ENTRYPOINT	#指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD		#当构建一个被继承DockerFile 这个时候就会运行 ONBUILD 的指令，触发指令
COPY		#类似ADD,将我们的文件拷贝至镜像中
ENV			#构建的时候设置环境变量
```

权限不够把自己加到docker里就不用每次都加sudo来执行docker命令了.

可以该/etc/group文件,也可以用`adduser XXX docker`命令
