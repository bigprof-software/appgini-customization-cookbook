# Convert 'Go to page' dropdown to a textbox

## Purpose of this recipe

The 'Go to page' dropdown is a way to navigate the various pages of a table in AppGini.
Users can open the dropdown and click on a pge number to navigate to that page. Here is how it looks like:

!['Go to page' dropdown](https://cdn.bigprof.com/screencasts/go-to-page-dropdown.gif)

While this is very useful for quick navigation between pages of a table, it might get cumbersome when there
is a large number of pages. For a table containing thousands or millions of records, such a dropdown would
become very painful to use.

A more user-friendly way in the case of huge tables is to have a text box where users can type the page number
and be able to navigate to it, without having to scroll through a painfully long dropdown. Here is how this would
work:

!['Go to page' dropdown](https://cdn.bigprof.com/screencasts/go-to-page-textbox-replacement.gif)

So, we're going to show the code for making this modification here.

## Files to edit

### hooks/footer-extras.php

```
<script>
  $j(function() {
    // get id of dropdown ({tablename}_pagesMenu)
    var id = location.href.match(/.*\/(.*)_view\.php/)[1] + '_pagesMenu';
    if(!$j('#' + id).length) return;

    var page = parseInt($j('#' + id).val());
    if(!page) page = 1;

    $j('#' + id).replaceWith(
      '<div class="input-group" style="width: 7em;">' +
        '<input type="text" id="' + id + '" class="form-control" value="' + page + '">' +
        '<span class="input-group-btn">' +
          '<button class="btn btn-default" type="button" id="go-to-page">' +
            '<i class="glyphicon glyphicon-ok"></i>' +
          '</button>' +
        '</span>' +
      '</div>'
    );

    $j('#go-to-page').on('click', function() {
      var frm = document.myform;

      frm.writeAttribute('novalidate', 'novalidate');
      frm.NoDV.value=1;
      frm.FirstRecord.value = parseInt($j('#' + id).val()) * 10 + 1;
      frm.submit();
    });
  })
</script>
```
