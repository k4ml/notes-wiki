Creating Django ManyToMany models with extra attributes.

```python
from kecupu.pos.models import Customer, Store, Order, OrderItem

# get a customer
c = Customer.objects.get(pk=1)

# we can create an order for this customer
# we can use store_id instead of passing Store model instance.
o = Order(customer = c, total=1, store_id=1)

# create OrderItem instance
oi = OrderItem.objects.create(item_id=1, order=o, qty=2)
o.save()
oi.save()
```
## Notes
