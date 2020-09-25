# Mint Linux常用记录

## 软件安装

Software Manager能安装chromium

### git

```bash
sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt install git
```

### PyCharm

添加“安装路径/bin”目录到PATH环境变量

```bash
vim ~/.bashrc
EXPORT PATH="xxx/bin:$PATH"
source ~/.bashrc
```

## 字体安装

```bash
# 将下载的字体解压放入
cd ~/Downloads
sudo unzip source-code-pro-2.030R-ro-1.050R-it.zip -d /usr/share/fonts/
# 进入目录,复制TTF字体文件
cd /usr/share/fonts/source-code-pro-2.030R-ro-1.050R-it/TTF
sudo mkdir /usr/share/fonts/source
sudo cp * /usr/share/fonts/source/
# 建立字体索引信息，更新字体缓存
sudo mkfontscale
sudo mkfontdir
sudo fc-cache
```

### Mariadb

#### 安装

Here are the commands to run to install MariaDB 10.4 from the MariaDB repository on your Ubuntu system:

```bash
sudo apt-get install software-properties-common
sudo apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc'
sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://mirror.one.com/mariadb/repo/10.4/ubuntu bionic main'
```

Once the key is imported and the repository added you can install MariaDB 10.4 from the MariaDB repository with:

```bash
sudo apt update
sudo apt install mariadb-server
```

使用 `mysql_secure_installation` 命令初始化
root:wuhaiyun

```sql
---- 创建数据库
CREATE DATABASE dota2;
---- 创建用户
CREATE USER 'dota2admin'@'%' IDENTIFIED BY 'wuhaiyun';
---- 授权用户
GRANT ALL ON dota2.* TO 'dota2admin'@'%';
flush privileges;
```

### 修改apt阿里源

```bash
apt update
apt upgrade
vim  /etc/apt/source.list
# 源文本
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse

apt update
apt upgrade
```