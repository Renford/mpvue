一、所需环境
 1. linux服务器 有外网访问权限,推荐centos7
 2. nginx
 3. docker docker-compose


二、环境配置
 1. linux服务器，阿里云

 2. 添加yum源:
 	curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo

 	curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

 	或手动修改: /etc/yum.repos.d/CentOS-Base.repo

    yum clean all
 	yum makecache

 3. 安装docker:

 	3.1 清理
 	sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine


    3.2 安装
    sudo yum install docker-ce docker-ce-cli containerd.io

    3.3 启动
    sudo systemctl start docker

    3.4 添加docker源
    sudo mkdir -p /etc/docker
	修改 /etc/docker/daemon.json,添加阿里dcoker源
	{
	  "registry-mirrors": ["https://l3rdwq1v.mirror.aliyuncs.com"]
	}

	sudo systemctl daemon-reload
	sudo systemctl restart docker

    3.5 测试
    sudo docker run hello-world


    *****因网络限制,可尝试阿里镜像源安装*****:
    # step 1: 安装必要的一些系统工具
	sudo yum install -y yum-utils device-mapper-persistent-data lvm2
	# Step 2: 添加软件源信息
	sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
	# Step 3: 更新并安装Docker-CE
	sudo yum makecache fast
	sudo yum -y install docker-ce
	# Step 4: 开启Docker服务
	sudo service docker start

 4. 安装docker-compose:
 	4.1 方案1 自选安装:
 	sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

 	sudo chmod +x /usr/local/bin/docker-compose

 	可增加操作: sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

 	4.2 方案2 下载安装:
 	https://dl.bintray.com/docker-compose/master/:docker-compose-Linux-x86_64

 	4.3 方案3 由于网络限制,可尝试手动安装:

 		4.3.1 安装pip
 		yum -y install epel-release
		yum -y install python-pip

		4.3.2 修改pip源
		修改 ~/.pip/pip.conf

		[global]
		index-url = https://mirrors.aliyun.com/pypi/simple/

		[install]
		trusted-host=mirrors.aliyun.com

		4.3.3 安装docker-compose
		pip install docker-compose
 
 5. 安装nginx

 	yum -y install nginx 
 	修改niginx配置 参见nginx文档

 6. 下载docker-compose.yml等镜像部属文件
 	1. 创建工作目录 mkdir -p /home/xx/xc-mp   

 	2. 下载部属相关文件
 	git clone https://code.aliyun.com/luckydzp/xc-mp-set.git

 	3. 执行 init_dir.sh 创建数据存储文件

 	4. 修改docker-compose中环境变量等配置信息

 	5. 执行 docker-compose up -d  完成部属

 7. 其它:
 	dbback.sh用于备份数据库


