---
title: "ASP.NET Snippets"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
# bookHref: ''
---
## ASP.NET : ASP.NET C#/Razor Snippets

```json
{
	"Create Razor Form": {
		"prefix": "razorform",
		"body": [
			"@using (Html.BeginForm(\"$1\", category, FormMethod.$2, new { @class = \"form\" }))",
			"{",
			"$0",
			"}"
		],
		"description": "Create a razor form"
	},

	"Create Razor Form With Route ID": {
		"prefix": "razorformid",
		"body": [
			"@using (Html.BeginForm(\"$1\", category, new { Id = @$2 }, FormMethod.$3))",
			"{",
			"$0",
			"}"
		],
		"description": "Create a razor form with ID route"
	},

	"Create Razor Label": {
		"prefix": "razorlabel",
		"body": [
			"@Html.LabelFor(m => m.$1, \"$2\")<br>"
		],
		"description": "Create a razor Label"
	},

	"Create Razor InputText": {
		"prefix": "razorinputtext",
		"body": [
			"@Html.TextBoxFor(m => m.$1)<br>"
		],
		"description": "Create a razor Input of type Text"
	},

	"Create Razor InputNumber": {
		"prefix": "razorinputnumber",
		"body": [
			"@Html.TextBoxFor(m => m.$1, new { @type = \"number\" })<br>"
		],
		"description": "Create a razor Input of type Number"
	},

	"Create Razor InputDate": {
		"prefix": "razorinputdate",
		"body": [
			"@Html.TextBoxFor(m => m.$1, \"{0:yyyy-MM-dd}\", new { @type = \"date\" })<br>"
		],
		"description": "Create a razor Input of type Date"
	},

	"Create Razor Input Hidden": {
		"prefix": "razorinputhidden",
		"body": [
			"@Html.HiddenFor(m => m.$1)"
		],
		"description": "Create a razor Input of type Hidden"
	},

	"Create Razor Dropdown": {
		"prefix": "razordropdown",
		"body": [
			"@Html.DropDownListFor(m => m.$1, @ViewBag.$2 as SelectList, ${3:null})<br />"
		],
		"description": "Create a razor dropdown"
	},

	"Create Razor Validator": {
		"prefix": "razorvalidator",
		"body": [
			"@Html.ValidationMessageFor(m => m.$1, \"\", new { @class = \"text-danger\" })<br>"
		],
		"description": "Create a razor Validation text feedback"
	},
}
```

