<script lang='ts'>
	import LibrarySnippet from './../components/LibrarySnippet.svelte';

  import InputCheckbox from '@form/InputCheckbox.svelte'

  let propstr = '';
  let selectstr = '';
  let inputstr = '';
  let props; 
  let selectprops;
  let inputprops;

  let eventLog = []
  const events = {
    onChange: (val) => {
      snippet.inputs[0].value = val;
      eventLog = [...eventLog, {action: 'onChange', val}]
    },
    onKeypress: (val) => {
      eventLog = [...eventLog, {action: 'onKeypress', val}]
    },
    updateForm: (val) => {
      eventLog = [...eventLog, {action: 'updateForm', val}]
    }        
  }

  $: snippet = {
    name: 'InputCheckbox',
    importName: '@form/InputCheckbox.svelte',
    props: {
      required: false,
      checked: true,
    },         
    inputs: [      
      {forprop: 'text', renderAs: 'input', componentprop: {type: 'text'}, value: 'Checkbox text goes here.' },
      {forprop: 'key', renderAs: 'input', componentprop: {type: 'text'}, value: null },
      {forprop: 'label', renderAs: 'input', componentprop: {type: 'text'}, value: 'label' },
    ],
    notes: [
      'key: prop is equivilent to element id.  Only need to alter if you have more then one Input on the same form.',
      'required: only triggers if wrapped in a Form component.  See Form component for more details.',            
    ]    
  }

  $: livecode = `    
    <InputCheckbox${propstr}${selectstr}${inputstr}/> 
     `


</script>


<LibrarySnippet {...snippet} {livecode} bind:propstr={propstr} bind:selectstr={selectstr} bind:inputstr={inputstr} bind:props={props} bind:selectprops={selectprops} bind:inputprops={inputprops} {events} {eventLog}>
  <div slot='liveexample'>    
    <InputCheckbox {...props} {...selectprops} {...inputprops} {...events} />
   </div>    
</LibrarySnippet>

<style lang='scss'>

</style>