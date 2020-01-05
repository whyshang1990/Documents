# Angular7
## 安装Angular7
### 安装node.js
https://nodejs.org/en/ 下载安装包，解压到目录
配置环境变量  `export PATH=$PATH:xxxxx`
修改源镜像
`npm set registry https://registry.npm.taobao.org/`
### 使用npm全局安装Angular CLI:
`npm install -g @angular/cli`

### 安装 ionic cordova
`npm install -g cordova ionic`

```
ionic start myApp tabs
# 运行项目:
$ cd myApp
$ ionic serve
# 打包
ionic cordova platform add android
# 检查安装环境
$ ionic cordova build android --release

/home/why/code/Projects/myApp/platforms/android/cordova/lib/builders/ProjectBuilder.js

修改
var distributionUrl = process.env['CORDOVA_ANDROID_GRADLE_DISTRIBUTION_URL'] || '../gradle-3.3-all.zip';



```

### 新建ionic4应用
`ionic start myApp tabs`

### angular命令
- 查看版本
`ng version`
- 创建项目
`ng new /home/why/code/Projects/AND`
- 安装内存Web API包
`npm install angular-in-memory-web-api --save`
- 创建内存API service
`ng generate service service/InMemoryData`
- 新建组件
`ng generate component heroes`
