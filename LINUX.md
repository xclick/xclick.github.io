# LINUX

## CentOS
### 问题解答
* **CentOS检查某软件包是否已安装**
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
  




