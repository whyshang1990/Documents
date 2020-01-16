# Angular7

## 安装Angular7

### 安装node.js

<https://nodejs.org/en/> 下载安装包，解压到目录
配置环境变量  `export PATH=$PATH:xxxxx`
修改源镜像
`npm set registry https://registry.npm.taobao.org/`

### 使用npm全局安装Angular CLI:

`npm install -g @angular/cli`

### 安装 ionic cordova

`npm install -g cordova ionic`

```bash
ionic start myApp tabs
# 运行项目:
cd myApp
ionic serve
# 打包
ionic cordova platform add android
# 检查安装环境
ionic cordova build android --release

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
- 打开开发服务器
`ng serve --open`

>ng serve 命令会构建本应用、启动开发服务器、监听源文件，并且当那些文件发生变化时重新构建本应用。  
--open 标志会打开浏览器，并访问 <http://localhost:4200/>

- 添加NG-ZORRO库

```bash
npm install ng-zorro-antd
ng add ng-zorro-antd
# 修复icons未导入的情况
```

- 安装内存Web API包
`npm install angular-in-memory-web-api --save`
- 创建内存API service
`ng generate service service/InMemoryData`
- 新建组件
`ng generate component heroes`

<https://blog.51cto.com/7308310/2072364>

## Angular配置

### 配置跨域请求

新建配置文件 proxy.conf.json，

```json
{
    "/":{
        "target":"http://localhost:8889/"
    }
}
```

修改package.json(使用 ng server --proxy-config proxy.config.json启用代理)

```json
"scripts": {
    "ng": "ng",
    "start": "ng serve --proxy-config proxy.config.json",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e"
  },
```

### 路由
在Angular App中添加路由模块
`ng generate module app-routing --flat --module=app`
> --flat 把这个文件放进了 src/app 中，而不是单独的目录中。
  --module=app 告诉 CLI 把它注册到 AppModule 的 imports 数组中。

## NG-ZORRO

### icons

- 静态引入icons，需要修改app.module.ts

```typescript
import { NZ_ICONS } from 'ng-zorro-antd/icon';
import { IconDefinition } from '@ant-design/icons-angular';
import { UserOutline } from '@ant-design/icons-angular/icons';

const icons: IconDefinition[] = [ UserOutline ];
registerLocaleData(en);

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    NgZorroAntdModule,
    FormsModule,
    HttpClientModule,
    BrowserAnimationsModule
  ],
  providers: [
    { provide: NZ_I18N, useValue: en_US },
    { provide: NZ_ICONS, useValue: icons }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- 动态引入icons，修改文件angular.json,在build下添加

```json
"assets": [
    ...
    {
        "glob": "**/*",
        "input": "./node_modules/@ant-design/icons-angular/src/inline-svg/",
        "output": "/assets/"
    }
],
```

### Form

使用响应式Form时，需要先导入模块（ReactiveFormModule）
`import { ReactiveFormModule } from '@angular/forms'`

表单组件的6种状态:

- valid 表单控件有效
- invalid 表单控件无效
- prisitine 表单控件值未改变
- dirty 表单控件值已改变
- touched 表单控件已被访问
- untouched 表单控件未被访问

#### [nz-form]指令

该指令用于元素`<form></form>`
响应式表单需在form元素上属性绑定`[formGroup]="xxxxxFormGroupProperty"`

#### nz-form-item 表单项

表单项用于区分表单中不同的区域，包含表单域（必选 nz-form-control）和表单标签（可选 nz-form-label）
