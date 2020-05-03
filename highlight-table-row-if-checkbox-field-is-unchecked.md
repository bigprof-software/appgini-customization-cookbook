# Highlight a table row if a checkbox field is unchecked

## Purpose of this recipe

You have a table in your AppGini app that includes a checkbox field. In the table view, you want to highlight records where this checkbox field is unchecked.

## Files to edit

### hooks/footer-extras.php

```
$j(function() {
    $j('.tablename-fieldname').find('.glyphicon-check').parents('tr').addClass('warning')
})
```
Replace `tablename` and `fieldname` in the above code with the name of the table and the checkbox field, respectively.
