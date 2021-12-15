# svelte-CRUD-Table

Lightweight CRUD table with no dependencies, reasonable functionality and no bloat.

## Features

- Lightweight with no dependencies & less than 200 LOC
- data is an array of objects
- configurable columns, show/hide, initial width, data type, column name
- supports display & edit of several data types - text, HTML, colour, date, select/option lists
- resizable column widths (any column can have resizing disabled)
- double click a row to edit (can be disabled)
- *Edit* & *Delete* buttons for each row (can be disabled)
- *Add Row* button (can be disabled)
- dispatchs events on edit, add row, delete row
- only a portion of the rows in the array can be displayed. This facilitates pagination by the parent.
- configurable text alignment for each column - left, center, right
- data is updated as soon as it is edited

## Not implemented

- Row sorting
- Hiding of columns by user
- Filtering of rows
- Pagination
- Styling
- Lazy loading
- NPM package. You'll probably want to style it yourself, so copy Table.svelte & svgIcon.js into your project.
- *Time* data type

Much of the above could be implemented by the parent.  

## Data Types

- text is just text
- html is displayed using {@html...}   **Be very careful**
- select has the value of the option
- date must be format YYYY-MM-DD
- colour must be format of #rrggbb

The above are all edited using <input type="xxx">  except for text & html which use <textarea>

## Example

```
<script>
    import Table from './Table.svelte';

    const table_config = {
        //  remove any of these to prevent that user action
        options: ['CREATE', 'EDIT', 'DELETE','DBLCLICKEDIT'],
        row_settings : {
            firstRow: 0,            //  pagination is implemented by parent
            lastRow:9999,
        },
        columns_setting: [
            {name: 'name', show: true, resizable:true, edit: true, width: 150, type:'text', align:'left', description:'Name'},
            {name: 'role', show: true, resizable:true, edit: true, width: 350, type:'html', align:'left', description: 'Role'},
            {name: 'email', show: true, resizable:true, edit: true, width: 350, type:'email', align:'right', description: 'Email'},
            {name: 'favCol', show: true, resizable:true, edit: true, width: 150, type: 'select', options:[], align:'center', description: 'Fav Colour'},
            {name: 'color', show: true, resizable:true, edit: true, width: 100, type: 'color', align:'center', description: 'Assigned Colour', },
            {name: 'dt', show: true, resizable:true, edit: true, width: 140, type: 'date', align:'center', description: 'Date', }
        ],
    };

    table_config.columns_setting[3].options = [
        {value:'r' , text:'Red'},
        {value:'g' , text:'Green'},
        {value:'b' , text:'Blue'},
        {value:'y' , text:'Yuck'}
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

    function onStartEdit(e) {console.log('start edit',e.detail);}
    function onEndEdit(e) {console.log('end edit ',e.detail);}
    function onAddRow() {console.log('addrow ',e.detail);}
    function requestdeleterow(e) {console.log('request delete row ',e.detail);}
</script>

    <Table
        config={table_config}
        bind:data={myData}
        on:startedit={onStartEdit}
        on:endedit={onEndEdit}
        on:addrow={onAddRow}
        on:requestdeleterow={(e)=>requestdeleterow(e)}
        />

```

## Events

Events dispatched -

- request delete of a row. The parent then deletes the row (or not).
- when editing of a row starts.
- when editing of a row finishes. This happens either when the user clicks the *Save* icon or clicks on a different row.
- when a row is created. The parent can then populate with default data.

The affected row number is passed in event.detail.  
See example above.  

## Select Options

A column can have type = 'select'.  This will allow the values to be selected from a dropdown list. The content and values of this select are specified in columns_setting[n].options[].  

The format is -  

```
options = [
    {value:'r' , text:'Red'},
    {value:'g' , text:'Green'},
    {value:'b' , text:'Blue'},
    {value:'y' , text:'Yuck'},
];
```

## Warning

Raw HTML may be displayed & edited using columns_setting[n].type = 'html'.  Sanitise it first or don't allow edit or don't use type='html'!  Or even better, delete that line of code from Table.svelte.
