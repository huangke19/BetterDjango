#### 在Form模型里重写clean方法



```python
class PageForm(forms.ModelForm):
    title = forms.CharField(max_length=128,
                            help_text="Please enter the title of page.")
    url = forms.URLField(max_length=200, help_text='Please enter the URL of the page')
    views = forms.IntegerField(widget=forms.HiddenInput(), initial=0)

    class Meta:
        model = Page
        exclude = ('category',)

    def clean(self):
        cleaned_data = self.cleaned_data
        url = cleaned_data.get('url')

        if url and not url.startswith('http://'):
            url = 'http://' + url
            cleaned_data['url'] = url

            return cleaned_data
```



为了对用户输入的数据进行检查，我们可以在ModelForm子类中覆盖clean()方法，这个方法在数据存为新模型实例之前调用，因此在这一时刻特别适合确认甚至修正用户在表单中输入的数据。



在clean()方法中，基本的处理过程是一样的，你可以套用这种方式，按照自己的方式处理表单数据。

- 表单数据从cleaned_date字典中获取
- 要检查的表单字段使用字典.get()方法从cleaned_data中获取，如果用户未在某个表单字段中输入值，那么cleaned_data字典中便没有对应的键值对，此时get()方法返回None而不抛出KeyError异常。
- 先检查要处理的表单字段中有没有值，如果有再检查值是什么，如果发现值不符合预期，那就做些处理，然后把新值存入cleaned_data字典
- clean()方法的最后必须返回cleaned_data字典的引用 ，否则不生效



clean()方法特别有用，尤其适合为字段提供默认值，或者为用户没有填写的字段提供值。

