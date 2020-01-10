# QtSql

## QSqlDatabase

**连接一个数据库**
构造函数

```python
QSqlDatabase.addDatabase("数据库驱动类型")
setDatabaseName() # sqlite3数据库只需要设置这个参数
open() # 打开一个数据库
```

## QSqlQueryModel

**提供一个只读的数据库查询结果模型**
可以用于为View类提供数据，例如：QTableView

```python
# 创建Model/View
self.table_view = QTableView()
self.query_model = QSqlQueryModel(parent=self)
# 设置Model Query，然后设置TableView展示的标题栏
self.query_model.setQuery("select cost,trans_type from tb_transactions LIMIT 7")
self.query_model.setHeaderData(0, Qt.Horizontal, '金额')
self.query_model.setHeaderData(1, Qt.Horizontal, '分类')

self.table_view.setModel(self.query_model)
```
