<script lang='ts'>
  import {getContext} from 'svelte';
  import Button from "@base/Button.svelte";
  
  export let links = ['dark', 'light']

  const theme:string = getContext('theme')

  const setTheme = (theme) => {
    localStorage.setItem('theme', theme)
    location.reload()
  }

</script>

<div class='themeswitcher' class:dark-theme={theme === 'dark'}>
  <Button exactfit applyTheme='white' size='small' disabled={theme === 'light'} hollow={theme === 'light'} onClick={() => {setTheme('light')}}>
    Light
  </Button>  

  <Button exactfit applyTheme='white' size='small' disabled={theme === 'dark'} hollow={theme === 'dark'} onClick={() => {setTheme('dark')}}>
    Dark
  </Button>    
</div>

<style lang='scss'>

  .themeswitcher{    
    justify-content: flex-start;
    display: flex;
    // gap: 5px;
    font-size: 12px;
    font-weight: 700;
    color: var(--black-1);

    &.dark-theme{
      color: var(--white-1);
    }
  }
</style>