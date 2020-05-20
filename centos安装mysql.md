wget https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm

rpm -ivh mysql57-community-release-el7-9.noarch.rpm
#yum -y localinstall mysql57-community-release-el7-11.noarch.rpm 

注意：必须进入到 /etc/yum.repos.d/目录后再执行以下脚本
#安装命令：
yum install mysql-server
#启动msyql：
systemctl start mysqld
service mysqld status
#初次查询并修改密码
grep 'temporary password' /var/log/mysqld.log
2020-05-20T05:39:41.182321Z 1 [Note] A temporary password is generated for root@localhost: w/acWTbue5fl
#无密码处理
rm -rf /var/lib/mysql
#重置密码
重置密码的第一步就是跳过MySQL的密码认证过程，方法如下
vim /etc/my.cnf(注：windows下修改的是my.ini)
在文档内搜索mysqld定位到[mysqld]文本段：
在[mysqld]后面任意一行添加“skip-grant-tables”用来跳过密码验证的过程，如下图所示：

SHOW VARIABLES LIKE 'validate_password%';
降低等级
set global validate_password_policy=LOW;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Adminzxc123.'; 
#mysql 密码策略相关参数；
1）、validate_password_length  固定密码的总长度；
2）、validate_password_dictionary_file 指定密码验证的文件路径；
3）、validate_password_mixed_case_count  整个密码中至少要包含大/小写字母的总个数；
4）、validate_password_number_count  整个密码中至少要包含阿拉伯数字的个数；
5）、validate_password_policy 指定密码的强度验证等级，默认为 MEDIUM；
关于 validate_password_policy 的取值：
LOW：只验证长度；
1/MEDIUM：验证长度、数字、大小写、特殊字符；
2/STRONG：验证长度、数字、大小写、特殊字符、字典文件；
validate_password_special_char_count 整个密码中至少要包含特殊字符的个数；
#修改密码
ALTER USER 'root'@'localhost' IDENTIFIED BY '@abcd123456'; 
set password=password("yourpassword"); 
#开启远程控制
update user set Host='%' where User='root'; 
flush privileges;
grant all privileges on 数据库名.表名 to 创建的用户名(root)@"%" identified by "密码";
grant all privileges on *.* to root@"113.123.123.1" identified by "123456789";
#开机启动
systemctl enable mysqld 
systemctl daemon-reload
#防火墙开放3306端口
firewall-cmd --zone=public --add-port=3306/tcp --permanent\
firewall-cmd --reload
#关闭开机启动
systemctl disable mysqld
#设置字符
配置默认编码为utf8：
vi /etc/my.cnf 
#添加 [mysqld] character_set_server=utf8 init_connect='SET NAMES utf8'
#查看编码
show variables like '%character%';
其他默认配置文件路径： 
配置文件：/etc/my.cnf 
日志文件：/var/log//var/log/mysqld.log 
服务启动脚本：/usr/lib/systemd/system/mysqld.service 
socket文件：/var/run/mysqld/mysqld.pid







