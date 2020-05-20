#创建用户
create user "zhuxiaoying"@"%" identified by "Zhuxiaoying";
#查看权限
show grants for "zhuxiaoying"@"localhost";

　　　　①. 指定某一个用户使用某一个ip登录并指定密码

　　　　　　create user "用户名"@"192.168.1.1" identified by "123";

　　　　②. 指定某一个用户使用某一网段的ip登录

　　　　　　create user "用户名"@"192.168.1. %" identified by "123";

　　　　③. 指定某一个用户可以使用任何ip登录

　　　　　　create user "用户名"@"%" identified by "123";

先设置该用户只有show database权限
grant select,insert,update,delete on redmine1.* to jira@"%" identified by "jira";


#自有执行
create user "sjfx"@"%" identified by "sjfx@.123";

grant select  on  ydyd.* to sjfx@"%" identified by "sjfx@.123";
show grants for "sjfx"@"localhost";
