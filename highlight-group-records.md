# Highlight records that belong to the current user's group
## Purpose of this recipe
There is no direct way for users of AppGini apps, when they are in
the table view, to tell which records belong to their group.
They need to go to the filters page and scroll down to **Records to display** where they can choose to view their group's records rather than all records.

In this recipe, we'll make this much quicker by highlighting the records that belong to the user's group directly in the table view, without having to go to the filters page first.

To achieve this, we need to have a _server-side_ PHP script that:

1. receives a list of record IDs, 
2. filters them to the ones that belong to the user's group,
3. and returns the filtered sub-list.

We also need to have _client-side_ JavaScript code that:

1. sends the list of record IDs displayed in the table view to that server-side PHP script (via an ajax request), 
2. waits for it to reply with the filtered sub-list,
3. and finally, highlight the table view records corresponding to the filtered sub-list.

## Example screenshots
Here is an example table view from the [Northwind demo](https://github.com/bigprof-software/northwind-demo/), with no record highlighting:
![](https://cdn.bigprof.com/screencasts/northwind-products-tv.png)

And here is the same page, after applying this recipe. The records
that belong to the current user's group are highlighted in light blue color:

![](https://cdn.bigprof.com/screencasts/northwind-products-tv-group-records-highlighted.png)

## Files to edit
Create a file in the `hooks` folder naming it `ajax-our-records.php` and add the following code to it (this is the server-side code for filtering list of record IDs to those owned by the current user's group):

```php
<?php
  /* script to receive list of ids of a table and
  return sublist of id owned by user's group */

  include(__DIR__ . "/../lib.php");
	
  // get current user groupID using getMemberInfo() function
  // @see: https://bigprof.com/appgini/help/advanced-topics/hooks/memberInfo-array
  $groupID = intval(getMemberInfo()['groupID']);

  // recieve table name and list of record IDs to check ownership for
  $table = $_REQUEST['table'];
  $ids = $_REQUEST['ids'];
  
  // ids must be an array
  if(!is_array($ids)) {
    @header('HTTP/1.0 400 Bad Request');
    exit;
  }

  // sanitize variables for query
  $table = makeSafe($table);
  $ids = array_map('makeSafe', $ids);
  $idsList = "'" . implode("','", $ids) . "'"; // returns "'id1', 'id2', 'id3', ..."

  // check membership_userrecords table for which records are owned by user's group
  $ownIds = []; $eo = ['silentErrors' => true];
  $res = sql(
    "SELECT `pkValue` FROM `membership_userrecords` 
     WHERE
       `tablename` = '$table' AND
       `pkValue` IN ($idsList) AND
       `groupID` = '$groupID'", $eo
  );
  while($row = db_fetch_row($res)) $ownIds[] = $row[0];

  @header('Content-type: application/json');
  echo json_encode($ownIds);
```

And add the following code to the `hooks/footer-extras.php` file (this is the client-side JavaScript code):

```js
<script>
  // highlight records owned by current user's group
  $j(function() {
    // if we're not in TV, no need to proceed
    var table = $j('.table_view').data('table');
    if(table === undefined) return;

    var ids = $j('tr[data-id]').map(function() { return $j(this).data('id'); }).get();
    if(!ids.length) return;

    $j.ajax({
      url: 'hooks/ajax-our-records.php',
      data: {
        table: table,
        ids: ids
      },
      success: function(ids) {
        if(!ids.length) return;
        $j('tr[data-id]').each(function() {
          if(ids.indexOf($j(this).data('id').toString()) >= 0)
            $j(this).addClass('info');
        })
      }
    })
})
</script>
```
