## Loading initial data (fixtures)

When running syncdb, it spitted out this error message:-
    ValueError: No JSON object could be decoded

Turn out the error in the json initial data file:-

```js
[
    {
        "pk": 1,
        "model": "auth.user",
    },
    {
        "pk": 1,
        "model": "app.profile",
    },
]
```
Notice the trailing comma ',' after the second object ?

## Custom views / object actions in admin
I want something like "History" button in the object edit form (change_form). Adding the button already covered in the docs but what not clear how to add my custom views that can operate on that object.

Override method `get_urls` in `ModelAdmin` subclass.
```python
    def get_urls(self):
        urls = super(WithdrawalAdmin, self).get_urls()
        my_urls = patterns('',
            (r'^(.+)/receipt$', self.receipt),
        )
        return my_urls + urls
```

but accessing the page as `/admin/myapp/withdrawal/2/receipt/` return 404 page saying `u"2/receipt"` primary key not exists. So the admin app took `2/receipt` as the object id instead of just `2` when try to load the object. This also mean even though I placed my custom urls first, the admin app still executing the edit form views. We need to override the `ModelAdmin.change_form` method:-

```python
    list_filter = ('status',)

    def change_view(self, request, object_id, extra_context=None):
        receipt = None
        try:
            pk_path = object_id.split('/')
            object_id = pk_path[0]
            receipt = True if pk_path[1] == 'receipt' else False
        except:
            pass

        if receipt:
            return self.receipt(request, object_id)

        return super(WithdrawalAdmin, self).change_view(request, object_id, extra_context)
```

The closest I found on stackoverflow:-
[[http://stackoverflow.com/questions/2805701/is-there-a-way-to-get-custom-django-admin-actions-to-appear-on-the-change-view]]

## Accessing object being edited in admin change_form template
[[http://stackoverflow.com/questions/4635548/accessing-the-object-in-a-django-admin-template]]