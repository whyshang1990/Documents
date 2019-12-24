# QtCore
### Qt
**包含Qt中使用的各种标识符，即常量**

### QRect
**定义一个平面矩形，通过指定左上角位置和宽高**
#### 构造函数
```python
QRect(QPoint topleft,  QPoint bottomright)
QRect(QPoint topleft, QSize size)
QRect(QRect)
QRect(left, top, width, height)
```
#### 常用方法
```python
# 获取宽高
width()
height()
```

### QPoint
**定义平面中的点，使用整数精度**
#### 构造函数
``` python
QPoint(QPoint)
QPoint(xpos, ypos)
QPoint() # 构造一个空点，即QPoint(0, 0)
```

### QSize
**定义二维对象的大小，使用整数精度**
#### 构造函数
``` python
QSize(QSize)
QSize(w, h)
```

