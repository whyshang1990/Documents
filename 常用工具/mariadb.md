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
