## 使用Slug为Django指定友好的URL



我们可以在URL中使用分类的唯一ID，例如/rango/category/1/或/rango/category/2/，数字12是分类的唯一ID，可是从ID上看不出到底是哪一个分类。



更好的方法是在URL中使用分类的名称，例如，可以通过/rango/category/python/访问与Python相关的网页列表。这样的URL简单、可读性高，而且具有一定的语义。



> 使用简洁且可读性高的URL是Web设计的重要一环。

为此，我们要使用Django提供的slugify函数



## 详细步骤

1. #### 为分类添加slug字段

   ```python
   class Category(models.Model):
       name = models.CharField(max_length=128, unique=True)
       views = models.IntegerField(default=0)
       likes = models.IntegerField(default=0)
       slug = models.SlugField()
   ```

2. #### 重写模型的save()方法

   ```python
   def save(self, *args, **kwargs):
               self.slug = slugify(self.name)
               super(Category, self).save(*args, **kwargs)
   ```

3. #### 执行迁移

   ```shell
   $ python manage.py makemigrations rango
   $ python manage.py migrate
   ```

4. #### 重新导入数据

   ```python
   python populate.py
   ```



## 问题解决



**我们没有为 slug 字段指定默认值，而且数据库中有数据，因此 migrate 命令会给你两个选择。选**
**择提供默认值的选项，输入一个空字符串(两个引号，即 '')**



此时添加新Category时，Django会让你手动填写slug字段，不然无法保存，解决方法

```python
slug = models.SlugField(blank=True)
```

这样可以自动保存别名了。比如**huang ke**将保存为**huang-ke**



上面的方法可以解决保存问题，但是这样容易出错，我们最好让django自动生成，为此我们可以在admin里进行定制

```python
class CategoryAdmin(admin.ModelAdmin):
    prepopulated_fields = {'slug': ('name',)}
```

为防止重复，可以在SlugField里添加 unique = True

