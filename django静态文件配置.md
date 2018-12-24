

## Django如何寻找静态资源



#### 定义静态资源文件夹

在settings.py顶部定义一个变量，STATIC_DIR

```python
STATIC_DIR = os.path.join(BASE_DIR, 'static')
```

这样得到一个绝对路径，指向项目根目录下static文件夹



#### STATICFILES_DIRS

定义好这个变量后，还要创建一个数据结构STATICFILES_DIRS，这个数据结构是一系列路径，让Django在其中寻找要伺服的静态文件，默认情况下，settings.py文件中没有这个列表。

```python
STATICFILES_DIRS = [STATIC_DIR,]
```



#### STATIC_URL

STATIC_URL变量指定启动Django开发服务器后通过什么URL访问静态文件

STATIC_DIR和STATICFILES_DIRS两个变量相当于服务器端的变量，而STATIC_URL相当于客户端访问静态内容的位置。