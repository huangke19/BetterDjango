在Django1.9中，TEMPLATES内容如下



```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

为了告诉Django模板在何处，要修改DIRS列表（默认为空）



## 动态路径

#### BASE_DIR

settings.py文件的顶部有一个BASE_DIR变量，它的值是settings.py文件所在的路径

```python
print(os.path.abspath(__file__))
print(os.path.dirname(os.path.abspath(__file__)))
print(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
```

```shell
$ /workspace/tango_with_django_project/tango_with_django_project/settings.py
$ /workspace/tango_with_django_project/tango_with_django_project
$ /workspace/tango_with_django_project
```



#### 自定义模板位置

```python
TEMPLATE_DIR = os.path.join(BASE_DIR, 'templates')
print(TEMPLATE_DIR)
```



为Django指定模板位置

```python
'DIRS': [TEMPLATE_DIR]
```



拼接路径一定要用os.path.join()函数，这样为的是使用正确的路径分隔符。Unix系操作系统使用正斜线/分隔符，Windows系统使用反斜线分隔符\，这样做也是为了避免硬编码。



#### 模板中使用静态文件

```python
{% load staticfiles %}
```

{% load staticfiles %}告诉Django模板引擎，将在模板中使用静态文件，这样便可以在模板中使用static模板标签引入静态目录中的文件，static标签会在路径前加上STATIC_URL指定的URL.



### 注意

HTML模板的第一行始终是DOCTYPE声明，如果把{% load staticfiles %}放在前面，渲染得到的HTML中，DOCTYPE声明前面将出现空白，多出的空白会导致HTML标记无法通过验证。