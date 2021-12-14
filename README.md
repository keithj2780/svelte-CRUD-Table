# svelte-CRUD-Table

## Features

- Lightweight with no dependencies & less than 200 LOC
- data is an array of objects
- configurable columns, show/hide, initial width, data type, column name
- supports display & edit of text, HTML, colour, date, select/option lists
- resizable column widths (any column can have resizing disabled)
- double click a row to edit (can be disabled)
- *Edit* & *Delete* buttons for each row (can be disabled)
- *Add Row* button (can be disabled)
- dispatchs events on edit, add row, delete row
- first row and last row to display are configurable. This allows pagination by the parent.

## Events

Events dispatched -

- request delete of a row. The parent then deletes the row (or not).
- when editting of a row starts
- when editting of a row finishes
- when a row is created. The parent can then populate with default data.

The affected row number is passed in event.detail

## Select Options

A column can have a type:'select'.  This will allow the values to be selected from a dropdown list. The content and values of this select are specified in columns_setting[n].options[].  

The format is -  

```
options:[
    {value:'r' , text:'Red'},
    {value:'g' , text:'Green'},
    {value:'b' , text:'Blue'},
    {value:'y' , text:'Yuck'},
]
```

## Example table configuration

```
	const table_config = {
        //  remove any of these to prevent that user action
		options: ['CREATE', 'EDIT', 'DELETE','DBLCLICKEDIT'],
        row_settings : {
            firstRow: 0,            //  pagination is implemented by parent
            lastRow:7,
        },
		columns_setting: [
			{name: 'name', show: true, resizable:true, edit: true, width: 150, type:'text',description:'Name'},
			{name: 'role', show: true, resizable:true, edit: true, width: 350, type:'html',description: 'Role'},
			{name: 'email', show: true, resizable:true, edit: true, width: 350, type:'email',description: 'Email'},
			{name: 'favCol', show: true, resizable:true, edit: true, width: 150, type: 'select', options:[], description: 'Fav Colour'},
			{name: 'color', show: true, resizable:true, edit: true, width: 100, type: 'color', description: 'Assigned Colour', },
			{name: 'dt', show: true, resizable:true, edit: true, width: 140, type: 'date', description: 'Date', },
		],
	};

    table_config.columns_setting[3].options = [
        {value:'r' , text:'Red'},
        {value:'g' , text:'Green'},
        {value:'b' , text:'Blue'},
        {value:'y' , text:'Yuck'},
    ];

```

## Example

```
<script>
	import Table from './Table.svelte';

	const table_config = {
        //  remove any of these to prevent that user action
		options: ['CREATE', 'EDIT', 'DELETE','DBLCLICKEDIT'],
        row_settings : {
            firstRow: 0,            //  pagination is implemented by parent
            lastRow:7,
        },
		columns_setting: [
			{name: 'name', show: true, resizable:true, edit: true, width: 150, type:'text',description:'Name'},
			{name: 'role', show: true, resizable:true, edit: true, width: 350, type:'html',description: 'Role'},
			{name: 'email', show: true, resizable:true, edit: true, width: 350, type:'email',description: 'Email'},
			{name: 'favCol', show: true, resizable:true, edit: true, width: 150, type: 'select', options:[], description: 'Fav Colour'},
			{name: 'color', show: true, resizable:true, edit: true, width: 100, type: 'color', description: 'Assigned Colour', },
			{name: 'dt', show: true, resizable:true, edit: true, width: 140, type: 'date', description: 'Date', },
		],
	};

    table_config.columns_setting[3].options = [
        {value:'r' , text:'Red'},
        {value:'g' , text:'Green'},
        {value:'b' , text:'Blue'},
        {value:'y' , text:'Yuck'},
    ];

	let myData = [
		{name:"Name 43 is Fred",email:"fred33@myco.com",role:"DevOps Engineer",favCol:"b",color:"#c99f84",dt:"2009-08-29"},
		{name:"Name 44 is Fred",email:"fred44@myco.com",role:"DevOps Engineer",favCol:"g",color:"#a0e3be",dt:"2011-09-27"},
		{name:"Name 45 is Fred",email:"fred@myco.com",role:"Application Developer",favCol:"r",color:"#c89aab",dt:"2000-05-10"},
		{name:"Name 46 is Fred",email:"fred@myco.com",role:"Data Entry",favCol:"g",color:"#89f0eb",dt:"1997-05-07"},
		{name:"Name 47 is Fred",email:"fred@myco.com",role:"UX Designer & UI Developer",favCol:"r",color:"#d7beb9",dt:"2001-08-05"},
		{name:"Name 48 is Fred",email:"fred@myco.com",role:"Web Developer",favCol:"y",color:"#c6a2a6",dt:"2003-08-13"},
		{name:"Name 49 is Fred",email:"fred@myco.com",role:"SQL Developer",favCol:"y",color:"#b4b6de",dt:"1999-02-27"},
		{name:"Name 50 is Fred",email:"fred@myco.com",role:"Web Designer",favCol:"b",color:"#d1d2ad",dt:"1997-07-27"},
		{name:"Name 51 is Fred",email:"fred@myco.com",role:"Application Developer",favCol:"r",color:"#febb87",dt:"2001-11-19"},	
	];
</script>

    <Table
        config={table_config}
        data={myData}
        on:startedit={onStartEdit}
        on:endedit={onEndEdit}
        on:addrow={onAddRow}
        on:requestdeleterow={(e)=>requestdeleterow(e)}
        />

```

## Warning

Raw HTML can be displayed using columns_setting[].type:'html'.  Sanitise it first!
