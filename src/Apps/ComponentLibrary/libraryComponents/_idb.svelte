<script lang='ts'>
  import {setContext} from 'svelte'
	import LibrarySnippet from './../components/LibrarySnippet.svelte';

  import IDB from '@core/IndexDBSetup.svelte'
  import {createDB} from '@js/utility'
  import {IndexDBStore} from '@js/index'

  let propstr = '';
  let selectstr = '';
  let inputstr = '';
  let props; 
  let selectprops;
  let inputprops;

  const snippet = {
    name: 'IDB',
    importName: '@core/IndexDBSetup.svelte',
    properties: `
    export let indexdb:any;
    export let data:any = []
    export let tables:string[] = []
    export let clearOnRefresh = false;
    export let queryBy:string[] | string = "_id"
    export let onReady = () => {}
    export let onUpdate = () => {}
    `,
    props: {
      clearOnRefresh: false
    }, 
  }

  $: livecode = `    
    const db = new IndexDBStore('example', 1); 
    // set context so it can be referenced in children components
    setContext('indexdb', db)

    let allUsers = [];
    let allPlaces = [];

    let query;

    const idb = createDB({
      indexdb: db,
      clearOnRefresh: true,
      tables: ['user', 'places'], 
      data: {
        user: [
          {name: 'Person 1', age: 18},
          {name: 'Person 2', age: 25},
          {name: 'Person 3', age: 32},
        ],
        places: [
          {name: 'Area 1', weather: 'warm'},
          {name: 'Area 2', weather: 'cold'},
          {name: 'Area 3', weather: 'chilly'},
        ]  
      }, 
      queryBy: 'name'
    })  

    const onReady = async() => {
      allUsers = await db.getAll('user');
      allPlaces = await db.getAll('places');
      query = await db.get('user', 'Person 2')    
    }

    <IDB {...idb} {onReady} ${propstr}${selectstr}${inputstr}/>
     `



  const db = new IndexDBStore('example', 1); 
  // set context so it can be referenced in children components
  setContext('indexdb', db)

  let allUsers = [];
  let allPlaces = [];

  let query;

  const idb = createDB({
    indexdb: db,
    clearOnRefresh: true,
    tables: ['user', 'places'], 
    data: {
      user: [
        {name: 'Person 1', age: 18},
        {name: 'Person 2', age: 25},
        {name: 'Person 3', age: 32},
      ],
      places: [
        {name: 'Area 1', weather: 'warm'},
        {name: 'Area 2', weather: 'cold'},
        {name: 'Area 3', weather: 'chilly'},
      ]  
    }, 
    queryBy: 'name'
  })  

  const onReady = async() => {
    allUsers = await db.getAll('user');
    allPlaces = await db.getAll('places');
    query = await db.get('user', 'Person 2')    
  }

</script>

<IDB {...idb} {onReady} />


<LibrarySnippet {...snippet} {livecode} bind:propstr={propstr} bind:selectstr={selectstr} bind:inputstr={inputstr} bind:props={props} bind:selectprops={selectprops} bind:inputprops={inputprops} >
  <div slot='liveexample'>    
    <p>★★ Check application/database</p>
    <hr>
    <h5>Users:</h5>
    {JSON.stringify(allUsers, null, 4)}     
    <hr> 
    
    <h5>Places:</h5>
    {JSON.stringify(allPlaces, null, 4)}     
    <hr>   
    
    <h5>Query:</h5>
    {JSON.stringify(query, null, 4)}     
   </div>    
</LibrarySnippet>


