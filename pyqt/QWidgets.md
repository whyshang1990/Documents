# QWidgets模块

## 重要类

### QApplication

**对于用Qt写的任何一个GUI应用，有且只有一个QApplication对象。**
从QGuiApplication继承，管理GUI程序的控制流和主要设置。
在新建一个QApplication对象后，方可创建QWidget，然后调用其show()或者exec()方法展示

- 一个完整应用的创建流程

```python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow

# 创建Qt应用
app = QApplication(sys.argv)
# 创建主窗口并展示
main_window = QMainWindow()
main_window.show()
# 调用app主循环，直到退出
sys.exit(app.exec_())
```

app.exec_()的作用是“进入程序的主循环直到exit()被调用”，如果没有这个方法，运行的时候窗口会闪退。所以show是有发挥作用的，但没有使用exec_()，所以没有进入程序的主循环就直接结束了。  
不用sys.exit(app.exec_())，只使用app.exec_()，程序一样可以正常运行，但是关闭窗口后进程却不会退出。

### QWidget

**控件，是所有用户界面对象的基类。**
没有嵌入父控件的控件，被称为窗口(window，top-level widget)，通常窗口包含框架和标题栏。
在Qt中，QMainWindow和QDialog是最常见的窗口类型

每个控件的构造函数，都接受1个或者2个参数：

窗口和子控件：
window(top-level widget): 没有父控件的控件，支持`setWindowTitle()，setWindowIcon()`方法设置标题栏和图标
Non-window(child widget)：Qt中大多数控件作为子控件使用

#### 常用方法

- setLayout(QLayout) 设置布局

### QLayout

**布局，是布局几何管理器的基类**，用于描述控件在应用程序用户界面中的布局方式
这是一个抽象的基类，具体的基础子类有：QHBoxLayout, QVBoxLayout, QGridLayout, and QFormLayout。
每个控件（QWidget）子类，使用布局管理器去管理他们的子类。使用`setLayout()`控件将应用布局。

#### Horizontal, Vertical, Grid, and Form Layouts

- **QHBoxLayout：** 将控件放于水平行中
- **QVBoxLayout：** 将控件放于竖直列中
- **QGridLayout：** 将控件放于二维网格中，一个控件可以占据多个单元格
- **QFormLayout：** 在2列描述性标签字段样式中设置控件。

使用`addLayout()`将布局内嵌，

#### 向布局添加控件

当向布局中添加控件时，布局处理过程如下：

1. 分配空间：根据控件的sizePolicy()和sizeHint()
2. 当拉伸系数(stretch factors)被设置大于0时：按照拉伸因子分配空间
3. 当拉伸系数(stretch factors)被设置为0时：只有在没有其他控件需要空间时，它们才能获取更多空间。其中，空间首先分配给具有扩展策略的控件
4. 尽量满足所有控件的最小尺寸(minimum size)、最小尺寸提示(minimum size hint)。（当不存在最小大小时，拉伸系数是决定因素）
5. 尽量满足控件的最大尺寸(maximum size)。（当不存在最小大小时，拉伸系数是决定因素）

#### 常用方法

- addWidget(QWidget) 添加控件
- setStretchFactor() 设置各个内嵌布局或者控件的拉伸系数，分配占用布局的占用比例
`setStretchFactor(QWidget, int), setStretchFactor(QLayout, int)`
- addStretch(int i) 添加一个拉伸空间(QSpacerItem)，分配占用布局空白空间的占用比列

#### 拉伸系数（stretch factors）

控件通常不设置默认拉伸系数。当控件在布局中时，根据他们的sizePolicy()，sizeHint()分配空间。**拉伸系数用于改变空间之间空间分配比例**。

#### 特定布局的使用

##### QGridLayout

**网格布局**

- addWidget() 添加控件方法提供额外参数
`(arg__1, row, column, rowSpan, columnSpan[, alignment=Qt.Alignment()])`

### QMainWindow

**提供主应用程序窗口。**
继承自QWidget，是一个顶层窗口，拥有自己的布局（Layout），包含很多常用的界面元素。

- 工具栏（QToolBar）
- 菜单栏（QMenuBar）
- 状态栏（QStatusBar）
- 中心窗口（center window）：默认由QWidget占位符占据
- 子窗口（QDockWidget）：包围着中心窗口

![img][main_window_layout]

#### MainWindow常用方法

```python
# 获取方法
menuBar()  # 获取默认的窗口菜单栏

# 设置
setCentralWidget(QWidget) # 设置中心窗口
setGeometry() # 设定在屏幕中的位置及大小
```

Qt中，**菜单**（QMenu）保存在**菜单栏**（QMenuBar）中，QAction被添加到**菜单**中，被显示为菜单项。setMenuBar()，setMenuWidget()

### QDeskWidget

**提供对各个系统桌面屏幕信息的访问**

#### 常用方法

```python
# 获取桌面几何信息，返回QRect对象
screenGeometry()
```

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

### QSpacerItem

**在布局中，提供一个空白控件**

## 重要控件类

### QTableWidget

**提供具有默认模型的基于项的表视图。**
表中的items封装成QtableWidgetItem.
表的行和列，可以由构造函数输入。或者可以在不指定大小的情况下构造表，然后调整大小。`setRowCount()，setColumnCount()`
将item插入表中，使用`setItem(row, column, item)`
表可以提供水平和垂直标题，使用`setHorizontalHeaderLabels()，setVerticalHeaderLabels()`

#### QtableWidgetItem

#### 常用方法

- slots
  - insertRow():

### QTableView

### QCalendarWidget

**提供一个基于月的日历部件，供用户选择日期**

#### 常用方法

```python
selectedDate() # 返回当前选择日期(QDate)
verticalHeader()/horizontalHeader() # 获取标题栏
```

#### QHeaderView

**为视图提供标题行或者标题列**

#### 常用方法

```python
hide()/setVisibal(True/False) # 设置其可见模式
```

### QLineWidget

### QGroupBox

**提供一个带有标题的分组框框架**

### QLabel

**提供文本或者图片展示，不提供用户交互功能**
从QFrame继承。
包含以下内容类型：

- 纯文本(Plain Text)：setText()
- 富文本(Rich Text)：setText()
- 像素图(pixmap)：setOixmap()
- 视频(movie)：setMovie()
- 数字(number)：setNum()

默认为左对齐，垂直居中（left-aligned, vertically-centered），可以调整通过`setAlignment(), setIndent()`调整位置

#### 常用方法

text(): 获取内容

### QPushButton


[main_window_layout]:data:image/png;base64,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