# Hide the 'Add new' (+) button beside a lookup field
## Purpose of this recipe
AppGini allows you to quickly add a parent record while editing a child record by clicking a button in the child form.

For example, let's say you have an orders table, and when you create a new order, you can specify the customer, which
is a lookup field with the parent being the customers table. And let's say you're creating an order for a new customer.
You don't have to first define the customer. Just start a new order, and from there, you can directly add a new customer.
See the screenshot below.

![](https://cdn.bigprof.com/screencasts/add-new-parent-from-child-dv.png)

This is quite convenient and time-saving. However, sometimes you don't want to display this *+* button.
This recipe tells you how to hide it.

## Files to edit
`hooks/footer-extras.php`

```
<script>
    $j(function() { $j('#customers_add_new).remove(); })
</script>
```

(replace `customers` above with the actual name of the parent table for the concerned lookup field)
