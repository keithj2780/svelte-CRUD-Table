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
            firstRow: 0,            //  pagination implemented by parent
            lastRow:99999,
        },
		columns_setting: [
			{name: 'name', show: true, resizable:true, edit: true, width: 150, description:'Name'},
			{name: 'bio', show: true, resizable:true, edit: true, width: 350, type:'html',description: 'Short bio'},
			{name: 'favCol', show: true, resizable:true, edit: true, width: 150, type: 'select', options:[], description: 'Fav Colour'},
			{name: 'color', show: true, resizable:true, edit: true, width: 100, type: 'color', description: 'Assigned Colour', },
			{name: 'dt', show: true, resizable:true, edit: true, width: 140, type: 'date', description: 'Date', },
		],
	};

    table_config.columns_setting.options[2] = [
        {value:'r' , text:'Red'},
        {value:'g' , text:'Green'},
        {value:'b' , text:'Blue'},
        {value:'y' , text:'Yuck'},
    ];


```

## Example

```
<script>
    let myData = [
        { name:'fred', dt:'2001/10/01', color:'#43d1f0', img:'http://ex.com/myimg.jpg',bio:'fred is great'},
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
