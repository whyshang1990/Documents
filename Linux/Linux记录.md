# Mint Linux常用记录
## 软件安装
Software Manager能安装chromium

### git
```
sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt install git
```

### PyCharm
添加“安装路径/bin”目录到PATH环境变量
```
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