# Redis

## Connecting

```python
import redis

r = redis.Redis(host='127.0.0.1', port=6390)
```

`host` and `port` default to `127.0.0.1` and `6379` respectively.

## Notes

`redis.Redis` implement `__setitem__` which aliased to `redis.Redis.set()` so it possible to set the key/value as:-

```python
r['key1'] = 'some value'

## equivalent
r.set('key1', 'some value')
```

