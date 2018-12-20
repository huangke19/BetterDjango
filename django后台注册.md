```python
from django.contrib import admin

from movie.models import Film, Category, Tag


# Register your models here.

@admin.register(Film)
class FilmAdmin(admin.ModelAdmin):
    list_display = ('name', 'category', 'desc', 'show_content')

    def show_content(self, obj):
        return obj.content[:40]

    show_content.short_description = '内容简介'

    pass

@admin.register(Category)
class CategoryAdmin(admin.ModelAdmin):
    list_display = ('name',)
    pass

@admin.register(Tag)
class TagAdmin(admin.ModelAdmin):
    list_display = ('name',)
    pass
```