# Highlight rows with due date = today
## Purpose of this recipe
You have a table storing some kind of task (could be an order, a task in a project, ... etc). This task has a due date, and you want to highlight records where due date is today.
This can be achieved by using JavaScript code that checks the due date in each displayed record and if that date is the same as today, applies a `danger` CSS class to the table row to highlight it.

## Example screenshot
![](https://cdn.bigprof.com/screencasts/appgini-highlight-row-if-duedate-today.png)

## Files to edit
Add the following code to `hooks/tablename-tv.js` (where tablename is the name of the concerned table). If the file doesn't exist, create it.

```
$j(function() {
  $j('td.tablename-due_date').each(function() {
    var td = $j(this),
        today = new Date,
        dueDate = moment(td.text().trim(), AppGini.datetimeFormat());

    if(!dueDate.isValid()) return;
    
    // if due date is today
    if(Math.abs(dueDate - today) < 86400000)
      td.parents('tr').addClass('danger');
  })
})
```
In the above code, replace `due_date` with the actual name of the due date field and `tablename` with the actual table name.
