# Add a shareable 'permalink' to records in detail view
## Purpose of this recipe

You can follow this recipe to display a link icon in the detail view of your data records. Users can share this link to
point directly to that specific record without having to open the table view first and select the record. So, this is
very useful for allowing users to share data via social media.

To allow anonymous users to see the shared record when they follow the link, you should configure the anonymous group
to view records (Admin area > Groups menu > Edit anonymous permissions).

## Files to edit

### hooks/tablename-dv.js
(Where tablename is the name of the concerned table). If that file doesn't exist, you should create it then add this code to it:

```js
$j(function() {
    // abort if new record
    if(!$j('[name=SelectedID]').val()) return;
    
    // get ID value of current record and url-encode it
    var id = encodeURIComponent($j('[name=SelectedID]').val());
    
    // build the shareable url, making sure to strip any extra parameters passed in it
    var url = location.href.replace(/\?.*/, '') + '?SelectedID=' + id;
    
    // insert the link to the right of the DV title
    $j('<a href="' + url + '" class="hspacer-md"><i class="glyphicon glyphicon-link"></i></a>')
       .insertAfter('.panel-title > strong');
}) 
```

