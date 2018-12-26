#### 在Django中创建数据驱动页面的流程



1. 在views.py文件中导入想使用的模型
2. 在视图函数中对模型进行处理，获取想呈现的数据
3. 把从模型获取的数据传给模型上下文/组织成指定的json格式
4. 创建或修改模板，显示上下文中的数据
5. 把URL映射到视图上

以上就是在Django中把模型、视图和模板连接在一起的步骤



#### 需求：在首页显示最受欢迎的5个分类

1. 在views.py文件中导入想使用的模型

   ```python
   from rango.models import Category
   ```

2. 在视图函数中对模型进行处理，获取想呈现的数据

   ```python
   most_liked_5_cats = Category.objects.order_by('-likes')[:5]
   ```

3. 把从模型获取的数据传给模型上下文/组织成指定的json格式

   ```python
   context_dict = {'categories': most_liked_5_cats}
   ```

4. 创建或修改模板，显示上下文中的数据

   ```html
   {% if categories %}
       <ul>
           {% for category in categories %}
               <li>{{ category.name }}</li>
           {% endfor %}
       </ul>
   {% else %}
       <strong>There are no categories present.</strong>
   {% endif %}
   ```

5. 把URL映射到视图上

   ```python
   url(r'^$', views.index, name='index'),
   ```
