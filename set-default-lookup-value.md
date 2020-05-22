# Select first item in a lookup dropdown by default
## Purpose of this recipe
You have a lookup field in a table. And when adding a new record to the table, you want that field to be set to
the first value by default (rather than being empty).

## Files to edit
Add the following code to `hooks/tablename-dv.js` (where tablename is the name of the concerned table).
If the file doesn't exist, create it.

```
$j(function() {
  // Run this code only if this is a new record
  if($j('[name=SelectedID]').val().length) return;

  $j.ajax({
	  url: 'ajax_combo.php',
    data: {
      s: '',
      p: 1,
      t: 'tablename',
      f: 'lookup_fieldname'
    },
    success: function(data) {
      if(data.results.length < 2) return;
      $j('#lookup_fieldname-container').select2('data', data.results[1]);
    }
  })
})
```

Change `tablename` and `lookup_fieldname` above to the actual table name and lookup field name.
