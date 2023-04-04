# 使用 Django、PostGIS 和传单的基于位置的应用程序

> 原文：<https://blog.devgenius.io/location-based-application-using-django-postgis-and-leaflet-9469845579c9?source=collection_archive---------4----------------------->

![](img/6adea266b10b3c3c3724db7aeaefdfd6.png)

一个空间指向另一个——水手大力水手。

在之前的一篇文章中，关于[混合空间索引](https://pyblog.medium.com/hybrid-spatial-data-structure-based-on-kd-tree-and-quadtree-8c0c5eebdbbf)查找空间数据点的实验是值得探索的，但是内存实现和缺乏伸缩能力使得在现实应用中使用它几乎毫无意义。

在一个需要存储和访问空间数据(坐标)的 web 应用程序中，Django、PostgreSQL (PostGIS)、GeoDjango 和 Leaflet(或 Google Maps)的组合解决了大多数初步用例。

# 当地的组织

*   安装依赖项
*   启动 PostgreSQL 服务器
*   配置 Django 应用程序

# 安装虚拟

和往常一样，在处理 python 项目时，总是使用`virtualenv`或`conda env`。如果你还没有安装`virtualenv`，安装它([参考](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/)):

```
python3 -m pip install --user --upgrade pip
python3 -m pip --version
python3 -m pip install --user virtualenv
```

# 安装依赖项

为项目创建一个环境，用适当的名称替换`env`:

```
python3 -m venv env
source env/bin/activate
python3 -m pip install -r requirements.txt
```

`requirements.txt`:

```
Django==4.0.2
django-leaflet==0.28.2
psycopg2-binary==2.9.3
gunicorn==20.1.0
```

当然，这里不需要`gunicorn`，只是一个练习，记住不要在生产中使用开发服务器(这是我经常在其他博文中看到的)。

# 安装必备组件

PostGIS 是 PostgreSQL 对象关系数据库的空间数据库扩展器。它支持地理对象，允许在 SQL 中运行位置查询。

```
brew install postgresql
brew install postgis
brew install gdal
brew install libgeoip
```

确保已经按照`requirements.txt`中所述安装了`psycopg2-binary`

# 启动 PostgreSQL 服务器

虽然您可以在本地安装`PostgreSQL`服务器，但是使用 docker 会使它更加容易，除了确保安装 docker 之外没有任何麻烦。

在本地机器上启动 docker 并运行:

```
docker run --name=postgis -d -e POSTGRES_USER=<database-username> -e POSTGRES_PASS=<database-password> -e POSTGRES_DBNAME=<database-name> -p 5432:5432 kartoza/postgis:14-3.2
```

并根据需要更换`<database-username>`和`<database-password>`、`<database-name>`。

# Django 项目设置

在`settings.py`中配置 PostGIS 数据库、INSTALLED_APPS 和传单

# 数据库

假设您手边已经有一个 Django 项目，在`settings.py`中，`database`:

```
DATABASES = {
    'default': {
        'ENGINE': 'django.contrib.gis.db.backends.postgis',
        'NAME': config('DATABASE_NAME'),
        'USER': config('DATABASE_USER'),
        'PASSWORD': config('DATABASE_PASSWORD'),
        'HOST': config('HOST_ENDPOINT'),
        'PORT': '5432',
    }
}
```

如果您使用的是 Mac M1，您需要添加到`gdal`的完整路径，对于本地设置，只需在`settings.py`中添加以下两个文件

```
GDAL_LIBRARY_PATH = '/opt/homebrew/opt/gdal/lib/libgdal.dylib'
GEOS_LIBRARY_PATH = '/opt/homebrew/opt/geos/lib/libgeos_c.dylib'
```

# 已安装的应用程序

将`settings.py`中的`django.contrib.gis`增加到`INSTALLED_APPS`。如果使用活页，安装`django-leaflet`并在`settings.py`中的`INSTALLED_APPS`处添加`leaflet`。

# 传单 _ 配置

在`settings.py`中增加`LEAFLET_CONFIG`配置

```
LEAFLET_CONFIG = {
    'DEFAULT_CENTER': (44.638569, -63.586262),
    'DEFAULT_ZOOM': 18,
    'MAX_ZOOM': 20,
    'MIN_ZOOM': 3,
    'SCALE': 'both',
    'ATTRIBUTION_PREFIX': 'Location Tracker'
}
```

最后，传单可能需要`static`；确保在`settings.py`中添加路径:

# 静电

根目录中静态文件的相对路径:

```
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

# Django 模型

假设您有一个名为`Trip`的模型，它有源地址和目的地址(坐标:纬度和经度)。

```
class Trip(models.Model):
    id = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)
    source_location = models.PointField(null=True)
    destination_location = models.PointField(null=True)
```

在`admin.py`中:

```
from leaflet.admin import LeafletGeoAdmin
from .models import Tripclass TripAdmin(LeafletGeoAdmin):
    list_display = ('source_location', 'destination_location')admin.site.register(Trip, TripAdmin)
```

# 运行迁移，创建超级用户，并启动开发服务器

```
python3 manage.py makemigrations
python3 manage.py migratepython3 manage.py createsuperuserpython3 manage.py runserver
```

最后，转到`http://localhost:8000/admin`，输入用户名和密码，并导航到具有位置字段的模型(`Trip`，以创建一个条目。

# 代码片段

在半径范围内搜索`source_location`的行程

```
from django.contrib.gis.geos import Point
from django.contrib.gis.measure import Distance
from .models import Tripdef get_trips(latitude, longitude, radius):
    point = Point(latitude, longitude)
    trips = Trip.objects.filter(source_location__distance_lt=(point, Distance(km=radius)))
    return trips
```

差不多就是这样！虽然这不是 GeoDjango 功能的完整列表，但请参考此处的文档:[https://docs . django project . com/en/4.0/ref/contrib/GIS/tutorial/#](https://docs.djangoproject.com/en/4.0/ref/contrib/gis/tutorial/#)了解更多详细信息。

参考代号:[https://github.com/addu390/dedo](https://github.com/addu390/dedo)一如既往