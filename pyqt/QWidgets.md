# QWidgets模块
## 重要类
### QApplication
从QGuiApplication继承，

### QWidget
**控件，是所有用户界面对象的基类。**
没有嵌入父控件的控件，被称为窗口(window，top-level widget)，通常窗口包含框架和标题栏。
在Qt中，QMainWindow和QDialog是最常见的窗口类型

每个控件的构造函数，都接受1个或者2个参数：

窗口和子控件：
window(top-level widget): 没有父控件的控件，支持`setWindowTitle()，setWindowIcon()`方法设置标题栏和图标
Non-window(child widget)：Qt中大多数控件作为子控件使用

### QLayout
**布局，是布局几何管理器的基类**，用于描述控件在应用程序用户界面中的布局方式
这是一个抽象的基类，具体的基础子类有：QHBoxLayout, QVBoxLayout, QGridLayout, and QFormLayout。
每个控件（QWidget）子类，使用布局管理器去管理他们的子类。使用`setLayout()`控件将应用布局。

#### Horizontal, Vertical, Grid, and Form Layouts
+ **QHBoxLayout：** 将控件放于水平行中
+ **QVBoxLayout：** 将控件放于竖直列中
+ **QGridLayout：** 将控件放于二维网格中，一个控件可以占据多个单元格
+ **QFormLayout：** 在2列描述性标签字段样式中设置控件。
使用`addLayout()`将布局内嵌，

#### 向布局添加控件
当向布局中添加控件时，布局处理过程如下：
1. 分配空间：根据控件的sizePolicy()和sizeHint()
2. 当拉伸系数(stretch factors)被设置大于0时：按照拉伸因子分配空间
3. 当拉伸系数(stretch factors)被设置为0时：只有在没有其他控件需要空间时，它们才能获取更多空间。其中，空间首先分配给具有扩展策略的控件
4. 尽量满足所有控件的最小尺寸(minimum size)、最小尺寸提示(minimum size hint)。（当不存在最小大小时，拉伸系数是决定因素）
5.尽量满足控件的最大尺寸(maximum size)。（当不存在最小大小时，拉伸系数是决定因素）

#### 常用方法
+ addWidget()

#### 拉伸系数（stretch factors）
控件通常不设置默认拉伸系数。当控件在布局中时，根据他们的sizePolicy()，sizeHint()分配空间。**拉伸系数用于改变空间之间空间分配比例**。

### QMainWindow
**提供主应用程序窗口。**
继承自QWidget，是一个顶层窗口，拥有自己的布局（Layout），包含很多常用的界面元素。
+ 工具栏（QToolBar）
+ 菜单栏（QMenuBar）
+ 状态栏（QStatusBar）
+ 中心窗口（center window）：默认由QWidget占位符占据
+ 子窗口（QDockWidget）：包围着中心窗口

```python
# MainWindow常用方法
menuBar()  # 返回默认窗口菜单栏
setCentralWidget(QWidget) # 设置中心窗口
```
Qt中，**菜单**（QMenu）保存在**菜单栏**（QMenuBar）中，QAction被添加到**菜单**中，被显示为菜单项。setMenuBar()，setMenuWidget()


### QMenuBar
**提供一个水平菜单栏**
菜单栏由下拉列表菜单项组成。使用addMenu()添加菜单项（QMenu）
对QMenu然后使用addAction()添加QAction
```python
# &表示按住ALT时可以使用的快捷键。即MainWindow激活时，使用ALT+F打开File菜单
file_menu = self.menuBar()..addMenu("&File")
```

### QMenu
**提供一个菜单控件，用于菜单栏，上下文菜单和其他弹出菜单**
菜单由操作项列表组成，有3种添加方式 addAction()、addActions()、insertAction()。
**注意：** 一个控件必须先添加Action，然后才能使用它
QMenu有四种操作项：分隔符，子菜单，控件，操作项，分别使用`addSeparator(),addMenu`添加分隔符和子菜单，使用addAction()添加操作项
添加操作项时，通常指定接收器和槽。没带被触发时，接收器将被通知。
QMenu默认有2种信号：triggered()和hovered()

### QAction
**提供一个抽象的用户操作界面，可以插入到控件中**
QAction可能包含图标（icon）、菜单文本（menu text）、快捷方式（shortcut）、状态文本（status text）、“What’s This?” text，和工具提示（tooltip）。
可以使用构造函数设置，也可以使用便捷方法设置`setIcon() , setText() , setIconText() , setShortcut() , setStatusTip() , setWhatsThis() , setToolTip()`
对于菜单项，可以使用`serFont()`设置字体
**注意：** 创建一个QAction之后，它应该被添加到相关的菜单和工具栏中，然后连接到执行该操作的槽。

## 重要控件类

### QTableWidget
**提供具有默认模型的基于项的表视图。**
表中的items封装成QtableWidgetItem.
表的行和列，可以由构造函数输入。或者可以在不指定大小的情况下构造表，然后调整大小。`setRowCount()，setColumnCount()`
将item插入表中，使用`setItem(row, column, item)`
表可以提供水平和垂直标题，使用`setHorizontalHeaderLabels()，setVerticalHeaderLabels()`

#### QtableWidgetItem

#### 常用方法
+ slots
  - insertRow():

### QLineWidget


### QLabel
**提供文本或者图片展示，不提供用户交互功能**
从QFrame继承。
包含以下内容类型：
+ 纯文本(Plain Text)：setText()
+ 富文本(Rich Text)：setText()
+ 像素图(pixmap)：setOixmap()
+ 视频(movie)：setMovie()
+ 数字(number)：setNum()
默认为左对齐，垂直居中（left-aligned, vertically-centered），可以调整通过`setAlignment(), setIndent()`调整位置

#### 常用方法
text(): 获取内容

### QPushButton


