<script lang='ts' context="module">

  export const exampleFormData = [
    {
      renderAs: 'input',      
      label: 'Firstname',
      placeholder: 'example: john smith',
      key: 'firstname',
      value: 'allen', 
      regex: /^[a-zA-Z.]+$/,      
      minLength: 3,
      maxLength: 25,
      required: true,
      sizing: {
        desktop: 25,
        mobile: 50
      }
    },
    {
      renderAs: 'input',      
      label: 'Lastname',
      placeholder: 'example: john smith',
      key: 'lastname',
      value: 'royston', 
      regex: /^[a-zA-Z.]+$/,      
      minLength: 3,
      maxLength: 25,
      required: true,
      sizing: {
        desktop: 25,
        mobile: 50
      }
    },
    {
      renderAs: 'input',      
      label: 'Username',
      placeholder: 'example: john smith',
      key: 'username',
      value: 'allen.royston', 
      regex: /^[a-zA-Z.]+$/,      
      minLength: 3,
      maxLength: 25,
      required: true,
      sizing: {
        desktop: 25,
        mobile: 50
      }
    },
    {
      renderAs: 'input', 
      type: 'password', 
      label: 'Password',
      key: 'password',
      value: 'notapassword',
      allowShowToggle: true,
      required: true,
      sizing: {
        desktop: 25,
        mobile: 50
      }
    },
    {
      renderAs: 'input',      
      type: 'number',
      label: 'Age',
      placeholder: 'example: john smith',
      key: 'age',
      value: '25', 
      regex: /^[0-9]+$/,      
      minLength: 1,
      maxLength: 3,
      required: true,
      sizing: {
        desktop: 25,
        mobile: 50
      }
    },
    {
      renderAs: 'input',       
      label: 'City',
      key: 'city',
      value: 'Reno',
      required: true,
      sizing: {
        desktop: 25,
        mobile: 50
      }
    },    
    {
      renderAs: 'textarea', 
      label: 'Description',
      key: 'description',      
      value: 'blah blah blah blah',
      required: true,
      contentEdit: true,
      minLength: 5,
      sizing: {
        desktop: 100,
        mobile: 100
      }      
    },
    {
      renderAs: 'date', 
      label: 'Date',
      key: 'date',
      value: '01-30-2021',
      minDate: '01-01-1950',
      maxDate: '01-01-2022',      
      required: true,
      sizing: {
        desktop: 50,
        mobile: 100
      } 
    },  
    {
      renderAs: 'time', 
      label: 'Time',
      key: 'time',
      value: '09:00',
      minTime: '07:00',
      maxTime: '22:00',      
      required: true,
      sizing: {
        desktop: 50,
        mobile: 100
      }       
    },     
    {
      renderAs: 'select', 
      label: 'Select',
      key: 'select',      
      value: 2,
      defaultOption: 'Select an option',
      options: [
        {id: 1, title: 'option 1'},
        {id: 2, title: 'option 2'},
        {id: 3, title: 'option 3'},
        {id: 4, title: 'option 4'}
      ],
      onInitFilter: (val, options) => {   
        return options.find(x => x.id === val)
      },
      onChangeFilter: (val) => {
        return val && val.id
      },
      required: true
    },      
    {
      renderAs: 'fileinput', 
      label: 'File Input (optional)',
      key: 'fileinput',      
      value: null,
      accept: 'image/png, image/jpeg'      
    },      
    {
      renderAs: 'selectmulti', 
      label: 'Multi Select',
      key: 'selectmulti',      
      value: [1, 3],      
      options: [
        {id: 1, title: 'option 1'},
        {id: 2, title: 'option 2'},
        {id: 3, title: 'option 3'},
        {id: 4, title: 'option 4'}
      ],
      onInitFilter: (arr, options) => {  
        return options.map((x) => {
          arr.forEach(v => {
            x._selected = !x._selected ? (x.id === v) : x._selected
          })
          return x
        })
      },
      onChangeFilter: (arr) => {           
        return arr.map(x => {
          return x.id
        })
      },
      required: true        
    },   
    {
      renderAs: 'rating',    
      label: 'Ratings',
      key: 'rating',      
      value: 2,                
      required: true,
      maxLength: 10  
    },    
    {
      renderAs: 'checkbox', 
      label: 'Agree',
      key: 'agree',
      text: 'Do you agree to the terms and agreements?',
      checked: true,
      required: true     
    },                                   
  ]

</script>


<script lang='ts'>  
  import {ModalStore} from '@store/store'
  import Form from '@form/FormBuilder.svelte'

  const {onModalSubmit} = ModalStore;

  export let onSubmit = (e) => {
    if(!!$onModalSubmit){
      $onModalSubmit(e)
    }
    else{
      isBusy = true      
      setTimeout(() => {
        alert(JSON.stringify(e))
        isBusy = false
      }, 2000)
    }
  };


  let isBusy = false

  const props = {
    formData: exampleFormData,
    onSubmit: () => {

    }, 
    idModifier: null,
    padding: 20,
    localStorageKey: null,
    clearLocalStorage: true
  }

</script>

<Form {...props} {isBusy} clearLocalStorage showButton />

