# Display a multiple choice field that includes an 'Other' user-provided option

## Purpose of this recipe

AppGini supports creating _option list_ fields, where users can select an option from a multiple choice list:

![Creating an options list field in AppGini](https://cdn.bigprof.com/screencasts/option-list-field.png)

which displays in the detail view form like this:

![Options list field in detail view form](https://cdn.bigprof.com/screencasts/option-list-field-dv.png?2)

Sometimes, we wish to add an **Other** option, allowing the user to type a different value, like this:

!['Other' option for a custom entry not in list](https://cdn.bigprof.com/screencasts/option-list-field-plus-other-dv.png)

The purpose of this recipe is to show a very easy way to convert a normal text field to a multiple choice list, with
an *other* option, where the user can type a different value if he wants.

**Note**: The field to which we'll apply this should be defined in AppGini as a *normal varchar/text field* rather than an *options list*.
The multiple choice options will be set in the code below.

## Files to edit

### `hooks/tablename-dv.js`

*Where tablename is the name of the table containing the field we want to apply this recipt to. Create that file if it doesn't exist.* 

```js
$j(function() {
	AppGini.textToOptions('ContactTitle', ['Mr', 'Mrs', 'Dr'], 'Other:');
})
```

Replace `ContactTitle` above with the actual field name, `'Mr', 'Mrs', 'Dr'` with the options you want to display,
and `Other:` with the label of the *other* option.

You can repeat line 2 if you have several fields where you want to apply this in the same table.
And of course, if you have multiple fields in multiple tables, you can apply this recipe to them by repeating
this step for each table.

### `hooks/footer-extras.php`

This step is needed just once, regardless of how many tables and fields we want to set
as multiple choice fields.

Here, we'll define the JavaScript function `AppGini.textToOptions()` that we used above, like this:

```js
<script>
AppGini.textToOptions = function(fn, options, otherLabel) {
	var inp = $j('#' + fn);
	if(!inp.length) return;
	if(inp[0].tagName.toLowerCase() != 'input') return;
	if(!options.length) return;
	if(!otherLabel) return;

	inp.addClass('hidden');

	// bool indicating if field val is not among provided options
	var otherChecked = options.indexOf(inp.val()) == -1;

	// append otherLabel if present to options
	options.push(otherLabel);

	// hide original input and show radio options
	options.reverse(false).map(function(label) {
		var i = options.indexOf(label) + 1;
		var optLabel = $j('<label></label>').css({ display: 'block', 'padding-top': '7px', margin: 0 }),
			opt = $j('<input type="radio">')
				.attr('id', fn + '-option-' + i)
				.attr('name', fn + '-options')
				.attr('value', label)
				.prop('checked', label == inp.val());

		opt.appendTo(optLabel);
		
		$j('<span></span>')
			.addClass('lspacer-sm rspacer-lg')
			.html(label)
			.appendTo(optLabel);

		optLabel.insertAfter(inp);
	});

	// get label container of last 'other' option
	var otherOpt = $j(`input[name="${fn}-options"]`).last(),
		otherOptContainer = otherOpt.parents('label');

	// inline 'other' text
	otherOptContainer.addClass('form-inline');

	// append 'other' textbox
	var otherText = $j('<input type="text">')
		.attr('id', fn + '-other-text')
		.addClass('form-control');
		
	otherText.appendTo(otherOptContainer);

	// select 'other' if field value is not among options provided
	otherOpt.prop('checked', otherChecked);

	// function to toggle 'other' textbox based on whether 'other' radio is set
	var updateOtherUI = function() {
		var otherChecked = otherOpt.prop('checked');
		otherText
			.prop('disabled', !otherChecked)
			.val(otherChecked ? inp.val() : '');
	}

	// handle changing 'other' radio and contents of 'other' text
	$j(`[name="${fn}-options"]`).on('change', function() {
		updateOtherUI();
		if(otherOpt.prop('checked')) {
			otherText.focus();
		} else {
			inp.val($j(this).val())
		}
	});
	otherText.on('change', function() {
		inp.val(otherText.val())
	})

	updateOtherUI();
}
</script>
```
