## 伺服媒体文件



#### 媒体文件和静态文件区别？

静态文件可以理解为不变的文件，媒体文件则是可能变化的文件，比如用户头像。



与静态文件一样，媒体文件也放在文件系统中专门的一个目录中，我们设为MEDIA_DIR

```python
MEDEA_DIR = os.path.join(BASE_DIR, 'media')
```

这一行的意思是，媒体文件将上传到Django项目根目录中的media目录里，把路径相关的变量放在settings.py模块的顶部便于以后修改。



在settings.py里添加两个变量

```python
MEDIA_ROOT = MEDIA_DIR
MEDIA_URL = '/media/'
```

MEDIA_ROOT变量告诉Django在哪里寻找上传的媒体文件，MEDIA_URL变量则指明通过什么URL伺服媒体文件。



#### 为MEDIA添加路由

修改urlpatterns列表，调用static参数

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    ...
    ...
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

到这里我们就可以通过 /media/URL访问media目录中的媒体文件了



#### 为模板添加MEDIA支持

找到settings.py文件中的TEMPLATES列表，里面嵌套context_processors列表，在context_processors列表中添加一个处理器，django.template.context_processors.media

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [TEMPLATE_DIR],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                'django.template.context_processors.media',
            ],
        },
    },
]
```

