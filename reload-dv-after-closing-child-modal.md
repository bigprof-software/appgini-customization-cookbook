# Reload detail view after closing child modal

## Purpose of this recipe

You have a child tab under one of your AppGini app forms.
After adding a new child record, or updating an existing one, you want to reload the main parent record.
See the screenshot below for the general workflow (supplied courtesy one an AppGini user who sent us this question).

![](https://cdn.bigprof.com/screencasts/reload-parent-dv-after-closing-child-modal.jpg)

## Files to edit

### hooks/tablename-dv.js
(where tablename is the name of the parent table -- create that file if it doesn't exist)

```js
$j(function() {
    $j(window).on('child-modal-closed', function(e, data) {
        $j('form').submit(); // reload the current DV
    })
})
```

#### More info about the above code
The above code utilizes the `child-modal-closed` event, which is triggered when a child
modal is closed. This event is available as of AppGini 5.90.
The `data` parameter is an object that contains details about the event as follows:

```js
data: {
    childTable: 'the name of the child table that triggered the event',
    childId: 'the ID of the child record that was just closed, or null if not available',
    parentTable: 'the name of the parent table',
    parentId: 'the ID of the parent record'
}
```
