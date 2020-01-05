# Django & Django REST Framework

## Django
### django-admin/manage.py
```
# 创建项目
django-admin startproject and
# 创建app
python manage.py startapp name
```
#### 移动app
在项目顶层，新建目录apps，创建app后，将其移动到apps目录下，
然后修改项目pythonpath，在settings.py文件内添加
```python
import sys
sys.path.insert(0, os.path.join(BASE_DIR, 'apps'))
```

## Django REST Framework

### 