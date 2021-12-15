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
			{name: 'dt', show: true, resizable:true, edit: true, width: 140, type: 'date', align:'center', description: 'Date', },
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

	function onStartEdit(e) {console.log('start edit',e.detail);}
    function onEndEdit(e) {console.log('end edit ',e.detail);}
    function onAddRow(e) {console.log('addrow ',e.detail);}
    function requestdeleterow(e) {console.log('request delete row ',e.detail);}
</script>

<main>
    <Table
        config={table_config}
        bind:data={myData}
        on:startedit={onStartEdit}
        on:endedit={onEndEdit}
        on:addrow={onAddRow}
        on:requestdeleterow={(e)=>requestdeleterow(e)}
        />
	<hr>
	<pre>
		{JSON.stringify(myData,null,2)}
	</pre>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	pre {
		color: #ff3e00;
		font-size: 1em;
		font-weight: 200;
		text-align: left;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>