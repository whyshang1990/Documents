# MariaDB

## Windows 10 安装

- 下载zip包，解压后进入bin目录
- 安装数据库`./mysql_install_db.exe`
- 执行命令`./nysqld.exe --install mariadb`新建服务,此处mariadb为启动服务名
`Service successfully installed.`表示安装成功
- 启动服务 `net start mariadb`(关闭服务`net stop mariadb`)

## Ubuntu 18.04 安装

### 安装

```bash
# 1. 安装 software-properties-common，如果缺少则进行安装
sudo apt-get install software-properties-common
# 2. 导入 MariaDB gpg 密钥：
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
# 3. 添加apt存储库
sudo add-apt-repository "deb [arch=amd64,arm64,ppc64el] http://mariadb.mirror.liquidtelecom.com/repo/10.4/ubuntu $(lsb_release -cs) main"
# 4. 在Ubuntu 18.04上安装MariaDB Server
sudo apt-get install mariadb-server
```

### 初始化

默认情况下，新安装的 Mariadb 的密码为空，执行非管理没权限的 mysql 命令可以出现异常。
使用 mysql_secure_installation 命令初始化。

### 设置默认字符集

修改 /etc/mysql/my.cnf
打开 my.cnf 后，在文档内的相应的位置增加如下几行代码:

```conf
[client]
default-character-set=utf8mb4
[mysqld]
character-set-server=utf8mb4
[mysql]
default-character-set=utf8mb4
```
service mysql restart


### 设置 MariaDB 的远程连接

```sql
-- 创建用户 wuhaiyun 指定密码 wuhaiyun '%'表示拥有访问权限的主机，%为通配符
create user wuhaiyun@'%' identified 'wuhaiyun';
-- 授权，all是指赋予所有权限，*.*是指所有的数据库和所有的表，后面是用户名和登录ip限制，
grant all on *.* to wuhaiyun@'%';
-- 刷新权限
flush privileges;
```

## brew 安装mariadb
可以使用mysql直接登陆
初始化root账号及数据库各种设定
`sudo mysql_secure_installation`

```bash
Enter current password for root (enter for none): <– 初次运行直接回车
…
Set root password? [Y/n] <– 是否设置root用户密码，建议设置，输入Y按回车
…
New password: <– 设置root用户的密码
Re-enter new password: <– 再输入一次你设置的root的密码
…
Remove anonymous users? [Y/n] <– 是否删除匿名用户，建议删除，Y删除，n不删除
…
Disallow root login remotely? [Y/n] <– 是否禁止root远程登录，按需配置，Y禁止远程登录，n开启远程登录
…
Remove test database and access to it? [Y/n] <– 是否删除test数据库，Y删除、n留着
…
Reload privilege tables now? [Y/n]<– 是否重新加载权限表，建议重新加载，Y重新加载，n不重新加载
…
All done! If you’ve completed all of the above steps, your MariaDB
installation should now be secure.
Thanks for using MariaDB!
```

创建用户
