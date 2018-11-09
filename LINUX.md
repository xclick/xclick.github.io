# LINUX
## Ubuntu
### 问题解答
#### Ubuntu 16.04安装Chrome浏览器
```bash
# 下载
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
# 安装
sudo dpkg -i google-chrome-stable_current_amd64.deb
# 出错： dpkg: status database area is locked by another process
sudo rm -rf /var/lib/dpkg/lock
# 出错： 
# dpkg: dependency problems prevent configuration of google-chrome-stable:
# google-chrome-stable depends on libappindicator1; however:
#  Package libappindicator1 is not installed.
sudo apt-get -f install libappindicator1 libindicator7
```


## CentOS
### 问题解答
#### CentOS检查某软件包是否已安装
  ```bash
  rpm -qa | grep "<Package Name>"
  ```
* **Linux中“is not in the sudoers file”解决方法**
  ```bash
  $su
  $vi /etc/sudoers
  # 找到"root    ALL=(ALL)       ALL"一行，在下面插入新的一行，内容是"pkc   ALL=(ALL)       ALL"
  $wq!
  $exit
  ```
* **升级系统**
  ```bash
  $sudo yum update
  # 重启
  $reboot  
  ```

* **安装nginx**
参考： https://www.ifshow.com/the-new-centos-7-install-lnmp-linux-nginx-mariadb-php-and-multi-site-configuration/
  ```bash
  # 安装EPEL源
  $yum -y install epel-release.noarch
  # 设置系统当前时区为上海，然后检查系统时区设置
  $timedatectl set-timezone Asia/Shanghai
  $timedatectl
  # 添加nginx官方库
  $rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
  # 安装Nginx
  $yum -y install nginx
  # 查看版本
  $nginx -v
  ```

* **nginx ssl**
  参考：http://www.jianshu.com/p/9523d888cf77   
  参考：https://cloud.tencent.com/document/product/400/4143  
  参考: https://docs.lvrui.io/2017/04/01/nginx%E9%85%8D%E7%BD%AEhttp%E5%BC%BA%E5%88%B6%E8%B7%B3%E8%BD%AChttps/
  文件参考:D:\Projects\PRIVATE\xclick.cn虚拟机
  * 去腾讯云上申请一个免费证书:xclick.cn
  * 新建:/etc/nginx/conf.d/https.conf
  * 将证书放到/etc/nginx下(不能放到)/etc/nginx/conf.d/下
  * 重启nginx
  * 在本机hosts中增加:192.168.1.32 xlick.cn
  * 在浏览器中访问:https://xclick.cn
  * http强制跳转https,用了rewrite 方法

* **nginx 转发 tomcat**
  参考: http://qtdebug.com/java-tomcat-nginx-https/

* **安装PHP**
参考： https://blog.csdn.net/liuhelong/article/details/79924014
  ```bash
  $yum install -y php php-mysql php-fpm
  # 验证安装
  $php --version
  # 启动php-fpm
  $systemctl start php-fpm
  ```

* **安装mariadb**
  ```bash
  $yum install -y mariadb mariadb-server
  ```  

* **卸载mariadb 安装mysql**
  参考：http://www.lx0758.cc/article/15.html
  ```bash
  # 列出安装的mariadb
  rpm -qa | grep mariadb
  # 卸载mariadb(强制)
  rpm -e –-nodeps xxxx
  # 安装wget
  yum install wget
  # 安装MySQL
  wget http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
  rpm -ivh mysql57-community-release-el7-10.noarch.rpm
  # 更新yum软件包
  yum check-update  
  # 更新系统
  yum update
  # 安装mysql
  yum install mysql-community-server
  # Root Password : P?8
  ```
* **安装ftp client**
  ```bash
  yum install ftp
  ```



* **CentOS 7中添加一个新普通用户并授权(who)**
  * https://zhuanlan.zhihu.com/p/27324580
  ```bash
  # 新增用户who
  adduser who
  passwd who
  password
  ```
* **关闭SELinux**
  ```bash
  # 检查SELinux状态
  sestatus
  # 临时关闭selinux 0是关闭 1是开启
  setenforce 0  
  #  永久关闭
  vi /etc/selinux/config
  #SELINUX=enforcing
  SELINUX=disabled
  ```  
* **How to replace firewalld with iptables in CentOS 7**
  参考： https://kompjuteras.com/en/how-to-replace-firewalld-with-iptables-in-centos-7/
  ```bash
  sudo systemctl stop firewalld
  sudo systemctl disable firewalld
  sudo yum -y install iptables-services
  sudo systemctl start iptables
  sudo systemctl start ip6tables
  sudo systemctl enable iptables
  sudo systemctl enable ip6tables
  # 配置 iptables
  vi /etc/sysconfig/iptables
  # 重启iptables
  systemctl restart iptables.service
  # 禁止iptables
  systemctl disable iptables
  # 暂停iptables
  systemctl stop iptables
  ```  
 * **修改SSH的远程访问端口**
  ```bash
  vi /etc/ssh/sshd_config
  # 修改为2121
  systemctl restart sshd
  # 修改防火墙
  vi /etc/sysconfig/iptables
  ``` 

* **安装JRE**
  参考：https://www.if-not-true-then-false.com/2014/install-oracle-java-8-on-fedora-centos-rhel/
  ```bash
  ftp 192.168.1.35
  # 进入binary mode
  ftp > binary
  ftp > get jre-8u144-linux-x64.rpm
  ftp > exit
  rpm -Uvh jre-8u144-linux-x64.rpm
  ```
* **安装tomcat**
  参考：http://www.jianshu.com/p/6a9fa018b506   
  配置为服务运行参考下面链接：  
  参考：https://www.howtoforge.com/tutorial/how-to-install-tomcat-on-centos/
  ```bash
  # 更新系统软件
  yum update
  # 创建用户，并加入用户组
  groupadd tomcat
  useradd -s /bin/bash -g tomcat tomcat
  # 下载tomcat 8，并上传至/usr/local文件夹
  cd /usr/local
  tar -zxvf apache-tomcat-8.5.20.tar.gz  
  # 修改权限
  chown -R tomcat:tomcat apache-tomcat-8.5.20
  # 改名
  mv apache-tomcat-8.5.20 tomcat
  # 启动
  cd /usr/local/tomcat
  ./bin/startup.sh
  # 作为服务启动
  # 将catalina.sh移至/etc/init.d/目录下，并更名为tomcat
  cd /bin
  cp catalina.sh /etc/init.d/tomcat
  # 修改comcat文件，并将之后的代码加入其中，保存并退出
  vi /etc/init.d/tomcat
  #本段代码加在#OS specific support................上输入
  #chkconfig:2345 10 90
  #description:Tomcat service
  CATALINA_HOME=/usr/local/tomcat
  JAVA_HOME=/usr/java/jre1.8.0_144
  # 修改权限
  chmod +x /etc/init.d/tomcat
  # 加入服务列表
  chkconfig --add tomcat
  # 检查是否加入列表
  chkconfig --list tomcat
  # 测试tomcat服务
  service tomcat
  # 启动服务
  service tomcat start
  # 设置管理
  admin/P?8
  ```  






