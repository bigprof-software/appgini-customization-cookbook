# Set date range for a datetime picker

## Purpose of this recipe

You have a datetime field in a table, and you want to limit allowed dates to a specific date range. For example, for 
the receipt dat/time field below, you want to limit users to specifying a date/time in the current year only.

![Example datetime field](https://cdn.bigprof.com/images/course/datetime-picker-example.png)

## Files to edit

### hooks/tablename-dv.js
(where `tablename` is the name of the table containing the datetime picker)

```
$j(function() {
 	// get the datetime picker -- replace datetime_field with the actual field name
	var dtp = $j('#datetime_field').parents('.input-group').data('DateTimePicker');
  
  // set the max date to end of current year
	dtp.maxDate(moment().endOf('year'));
  
  // and min date to the start of the current year
	dtp.minDate(moment().startOf('year'));
})
```

Now, when a user opens the date picker and tries to navigate to a date outside current year,
it would be disabled:

![Disabled dates](https://cdn.bigprof.com/images/course/datetime-picker-selection-limits.png)
