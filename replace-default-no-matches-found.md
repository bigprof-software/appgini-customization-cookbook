# Replace 'No matches found' with a custom message
## Purpose of this recipe
When using quick search in an AppGini app, and no matches are found, a default `alert-warning` message is displayed.
We want to replace it with some custom HTML message. See the screenshot below.
![](https://cdn.bigprof.com/screencasts/appgini-replace-no-matches-found.png)

## Files to edit
Add the following code to `hooks/tablename-tv.js` (where tablename is the name of the concerned table).
If the file doesn't exist, create it.

```
$j(function() {
   $j('.table_view .alert-warning').replaceWith('<div>This search term was not found!</div>');
})
```
In the above code, replace `<div>This search term was not found!</div>` with the
desired HTML code to display.

## Notes
If you'd like to apply this code to all tables rather than just a specific one,
you can add the code to `hooks/footer-extras.php` instead of `hooks/tablename-tv.js`.
In that case you should add `<script>` before the code, and `</script>` after it.
