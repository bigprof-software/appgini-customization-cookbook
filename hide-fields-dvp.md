# Hide specific fields from the detail view print preview
## Purpose of this recipe
You want to hide one or more fields when printing the detail view of some table in your AppGini app.

## Files to edit
Add the following code to `hooks/tablename-dv.js` (where tablename is the name of the concerned table).
If the file doesn't exist, create it.

```
$j(function() {
   // set the contents of this array to the indexes of fields to hide
   // 1st field is 0, 2nd is 1, ... etc.
   var hide = [2, 5, 7];
  
   // don't edit below here!
   // execute this only in print-preview
   if(!$j('#print').length) return;
   hide.map(function(i) { return $j('.form-group').eq(i).addClass('hidden'); });
})
```

Change the numbers in line 4 above to the indexes of the fields you want to hide.
