

### django表单处理流程整理



1. 创建forms.py文件

2. 为模型定义表单类

3. 为表单定义指定的字段

   ```python
   class CategoryForm(forms.ModelForm):
       name = forms.CharField(max_length=128,
                              help_text='please enter the category name.')
       views = forms.IntegerField(widget=forms.HiddenInput(), initial=0)
       likes = forms.IntegerField(widget=forms.HiddenInput(), initial=0)
       slug = forms.CharField(widget=forms.HiddenInput(), required=False)
   
       class Meta:
           model = Category
           fields = ('name',)
   
   
   class PageForm(forms.ModelForm):
       title = forms.CharField(max_length=128,
                               help_text="Please enter the title of page.")
       url = forms.URLField(max_length=200, help_text='Please enter the URL of the page')
       views = forms.IntegerField(widget=forms.HiddenInput(), initial=0)
   
       class Meta:
           model = Page
           exclude = ('category',)
   ```

4. 在视图中使用表单捕获用户提交的数据

   - 显示表单

   - 保存表单数据

   - 对数据进行校验

     ```python
     def add_category(request):
         if request.method == 'POST':
             form = CategoryForm(request.POST)
             if form.is_valid():
                 form.save(commit=True)
                 return index(request)
             else:
                 form = CategoryForm()
                 print(form.errors)
         else:
             form = CategoryForm()
         return render(request, 'rango/add_category.html', {'form': form})
     ```

5. 在模型里显示表单

6. 添加URL映射

   ```python
   return render(request, 'rango/add_category.html', {'form': form})
   ```
