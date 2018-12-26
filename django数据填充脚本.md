

导入Django模型之前要导入django，并把环境变量DJANGO_SETTINGS_MODULE设为项目的设置文件，然后调用django.setup()，导入Django项目的设置，如果缺少这一步，导入模型时会抛出异常，国为所需的Django基础设施未初始化。



Get_or_create()方法返回一个元组(object, created)，第一个元素是数据库中不存在记录时创建的模型实例引用，第二个元素是布尔值，创建实例时为True，反之为False



#### 填充脚本

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

'''
1. 检查先决条件
2. 定义子程序要解决的问题
3. 为子程序命名
4. 决定如何测试子程序
5. 在标准库中搜寻可用的功能
6. 考虑错误处理
7. 考虑效率问题
8. 研究算法和数据类型
9. 编写伪代码
    1. 首先简要地用一句话来写下该子程序的目的，
    2. 编写很高层次的伪代码
    3. 考虑数据
    4. 检查伪代码
10. 在伪代码中试验一些想法，留下最好的想法
'''

import os

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'tango_with_django_project.settings')

import django

django.setup()
from rango.models import Category, Page


def populate():
    # 首先创建一些字典，列出想添加的各分类的网页
    # 然后创建一个嵌套字典，设置各分类

    python_pages = [
        {"title": "Official Python Tutorial",
         "url": "http://docs.python.org/2/tutorial/"},
        {"title": "How to Think like a Computer Scientist",
         "url": "http://www.greenteapress.com/thinkpython/"},
        {"title": "Learn Python in 10 Minutes",
         "url": "http://www.korokithakis.net/tutorials/python/"}
    ]
    django_pages = [
        {"title": "Official Django Tutorial",
         "url": "https://docs.djangoproject.com/en/1.9/intro/tutorial01/"},
        {"title": "Django Rocks",
         "url": "http://www.djangorocks.com/"},
        {"title": "How to Tango with Django",
         "url": "http://www.tangowithdjango.com/"}]
    other_pages = [
        {"title": "Bottle",
         "url": "http://bottlepy.org/docs/dev/"},
        {"title": "Flask",
         "url": "http://flask.pocoo.org"}]
    cats = {"Python": {"pages": python_pages},
            "Django": {"pages": django_pages},
            "Other Frameworks": {"pages": other_pages}}
    # 如果想添加更多分类或网页，添加到前面的字典中即可
    # 下述代码迭代 cats 字典，添加各分类，并把相关的网页添加到分类中
    # 如果使用的是 Python 2.x，要使用 cats.iteritems() 迭代
    # 迭代字典的正确方式参见
    # http://docs.quantifiedcode.com/python-anti-patterns/readability/

    for cat, cat_data in cats.items():
        c = add_cat(cat)
        for p in cat_data['pages']:
            add_page(c, p['title'], p['url'])

    for c in Category.objects.all():
        for p in Page.objects.filter(category=c):
            print("- {0} - {1}".format(str(c), str(p)))


def add_cat(name):
    c = Category.objects.get_or_create(name=name)[0]
    if name == 'Python':
        c.views = 128
        c.likes = 64
    elif name == 'Django':
        c.views = 64
        c.likes = 32
    else:
        c.views = 32
        c.likes = 16
    c.save()
    return c


def add_page(cat, title, url, views=0):
    p = Page.objects.get_or_create(category=cat, title=title)[0]
    p.url = url
    p.views = views
    p.save()
    return p


if __name__ == '__main__':
    print("Starting Rango population script...")
    populate()
```

