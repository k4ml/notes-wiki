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