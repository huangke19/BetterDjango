# BestPrcatice


## Models

### M is bigger than V and C

Rule of Representation: Fold knowledge into data so program logic can be stupid and robust.

Once you have a model, you can rapidly derive models admins, models forms, and all kinds of generic views.



### Splitting models.py into multiple files

Like most components of Django, a large models.py file can be split up into multiple files within a package.

All definitions that can be exposed at package level must be defined in \__init__.py with global scope.

```python
# example

models/
|	 __init__.py
|	comment.py
|	post.py
|	postable.py
```

To ensure that all the models are imported correctly, \__init__.py should have the following lines:

```python
from post import Post
from postable import Postable
from comment import Comment
```



### Abstract inheritance

P: distinct models have the same fields and/or methods duplicated violating the DRY principle

S: Extract common fields and methods into various reusable model mixins

```python
# example

class Postable(models.Model):
    created = models.DateTimeField(auto_now_add=True)
    modified = models.DateTimeField(auto_now=True)
    message = models.TextField(max_length=500)
  	
    class Meta:
        abstract = True		
        
class Post(Postable):
    ...
    
class Comment(Postable):
    ...
```

to turn a model into an abstract base class, you will need to mention abstract=True in its inner Meta class



### Model mixins

Model mixing are abstract classeds that can be added as a parent class of a model. Python supports multiple inheritances. you can list any number of parent classes for a model.

Mixing ought to be orthogonal and easily compassable. they are more similar in behavior to composition rather than inheritance.

Small mixins are better. Whenever a mixin becomes large and violates the single responsibility priciple, consider refactoring it into smaller classes. Let a mixing do one thing and do it well.

```python
# refactory class Postable

class TimeStampedModel(models.Model):
    created = models.DateTimeField(auto_now_add=True)
    modified = models.DateTimeField(auto_now=True)
    
    class Meta:
        abstact = True
    
class Postable(TimeStampedModel):
    ...
    message = models.TextField(max_length=500)
  	
    class Meta:
        abstract = True		
        
class Post(Postable):
    ...
    
class Comment(Postable):
    ...
```







## split settings.py

settings文件应该拆分成一个包，根据不同的环境使用不同的配置，比如dev.py，prod.dv，test.py之类



## 视图函数要简洁

 视图函数应该用于控制应用逻辑，数据操作应该尽可能的放入模型

Ideally, your views should not contain anything other than presentation logic.



## 复杂的view和models拆分成包

功能耦合紧密的需求规划入一个App内，在app内部，逻辑相近的功能放在一起，这样，一个views.py或models.py就不能满足需求，需要拆成一个views包或者models包



## 使用Sphinx自动生成文档 

文档不一定要自己写，可以使用Sphinx自动生成，同时这就要求你的每一个函数和类需要有规划和详尽的注释文档



## 对具有通用属性的模型定义基类

如果多个model都具有相同的属性，可以考虑定义一个基类，将通用属性放入基类，然后继承之



## 常量和异常统一放入配置文件

常量使用大写，异常code_msg组织成一个字典，统一存放