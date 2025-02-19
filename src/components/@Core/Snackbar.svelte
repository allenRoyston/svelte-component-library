<script lang="ts">
//--------------------------- IMPORTS  
import { getContext, onDestroy } from 'svelte';
import SVG from '@base/SVG.svelte'
import Loader from '@base/Loader.svelte';

export let snack = null;
export let onComplete = () => {};
export let onClick = () => {}

const colors:any = getContext('colors')
const theme:string = getContext('theme');

let currentSnack = null;
let eventTimers = [];
let removeTimers = [];
let snacks = []

//--------------------------- ONMOUNT
onDestroy(() => {
  eventTimers.forEach(et => {
    clearTimeout(et.timer)
  })

  removeTimers.forEach(timer => {
    clearTimeout(timer)
  })
})
//---------------------------     

//--------------------------- EVENT HANDLERS
const processSnack = () => {
  snacks = snacks.map((s, i) => {
    if(!s.processed){
      if(s?.duration && s.duration > 1000){
        const timer = setTimeout(() => {
          s.animateIn && animateOutSnack(s)          
        }, s.duration)
        eventTimers = [...eventTimers, {id: s.id, timer}]
      }
    }
    return s
  })
}

const animateOutSnack = (snack, force = false) => {  
  if(snack?.duration || force){
    snacks = snacks.map(x => {
      if(x.id === snack.id){
        x.animateIn = false;
      }
      return x
    })
    const removeTimer = setTimeout(() => {       
      clearTimeout(removeTimers[0])
      removeSnack(snack)
    }, 200)  
    removeTimers = [...removeTimers, removeTimer]     
  }     
}

const cancelTimer = (id) => {  
  snacks = snacks.map(x => {
    if(id === x.id){
      x.duration = null
    }
    return x
  })
  const event = eventTimers.find(x => x.id === id)  
  event?.timer && clearInterval(event.timer)  
}

const removeSnack = (snack) => {
  snacks = snacks.filter(s => {return s.id !== snack.id})  
  eventTimers = eventTimers.filter(t => {return t.id !== snack.id})
  onComplete()
}

const newSnack = async() => {
  const id = Math.random().toString(36).substring(7);

  const progress = 50

  snack = {...snack, processed: false, animateIn: true, progress, id}
  currentSnack = snack;
  snacks = [...snacks, snack]
  processSnack()
}
//---------------------------

const returnIconColor = (type) => {
  switch(type){
    case 'primary': 
    return colors.primary[0].color
    case 'success': 
    return colors.success[0].color
    case 'warning': 
    return colors.warning[0].color
    case 'danger': 
    return colors.danger[0].color
  }  
}

const returnIcon = (type) => {
  switch(type){
    case 'primary': 
    return 'notification'    
    case 'success': 
    return 'like'
    case 'warning': 
    return 'warning'
    case 'danger': 
    return 'sad'
  }  
}


//---------------------------
$: {
  snack !== currentSnack && newSnack()    
}
//---------------------------

</script>

<div class={`snackbar`} >
  {#each snacks as snack (snack.id)}
    <div 
      class={`snack ${!!theme ? `${theme}-theme` : ''} ${snack?.applyTheme || ''} ${snack?.closeOnClick ? 'clickable' : ''}`} 
      class:animateIn={snack.animateIn} 
      class:animateOut={!snack.animateIn}
      class:btmpadding={snack?.duration}
      on:click={snack?.closeOnClick ? () => {removeSnack(snack)} : onClick} 
      on:mouseenter={() => {cancelTimer(snack.id)}} >

      <div class='icon'>
        <SVG icon={returnIcon(snack.applyTheme)} fill={returnIconColor(snack.applyTheme)} />
      </div>
      
      <div class='content' class:content-padding={snack?.component}>
        {#if snack?.message}
          {snack.message}
        {/if}
        {#if snack?.component}
          <svelte:component this={snack?.component} {...snack?.props} />
        {/if}
      </div>
    
      <div class='close-btn' on:click={() => {animateOutSnack(snack, true)}}>
        <SVG icon={snack?.duration ? 'unlocked' : 'cross' } fill={returnIconColor(snack.type)} size={12} />
      </div>

      {#if snack?.duration}
        <div class='progress-bar'>
          <Loader show type={snack.type} />
        </div>
      {/if}

    </div>
  {/each}
</div>


<style lang='scss'>
  @import '../../scss/src/_media-queries.scss';

  .snackbar{
    position: fixed;

    z-index: 100;  
    overflow: hidden;  
    display: flex;
    flex-direction: column-reverse;    
    padding: 0 5px;
    width: calc(100% - 10px);
    right: 0;
    bottom: 0;

    @include desktop-and-up {  
      padding: 0;
      bottom: 10px;
      right: 10px;      
      width: auto;
    }    

  }

  .snack{    
    padding-left: 15px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    border-radius: 5px;
    min-width: 150px;
    font-size: 12px;
    overflow: hidden;
    background: var(--black-1);   

    @include desktop-and-up {  
      width: auto;
    }         

    &.btmpadding{
      padding-bottom: 4px;
    }

    &.clickable{
      cursor: pointer
    }

    &:hover{
      background: var(--black-2);
    }

    &.primary{
      color: var(--primary-0);
    }

    &.success{
      color: var(--success-0);
    }

    &.warning{
      color: var(--warning-0);
    }   

    &.danger{
      color: var(--danger-0);
    }    

    &.dark-theme{
      background: var(--black-0);    


      &:hover{
        background: var(--black-2);    
      }

      &.primary{
        color: var(--primary-0);
      }

      &.success{
        color: var(--success-0);
      }

      &.warning{
        color: var(--warning-0);
      }   

      &.danger{
        color: var(--danger-0);
      }   
    }


    &:not(:last-child){
      margin-top: 10px;
    }

    &.animateOut{
      opacity: 0;
      transform: translateX(100%);
      transition: 300ms;
    }
    
    &.animateIn{
      opacity: 1;
      transform: translateX(0);
    }

    .progress-bar{
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 5px;

      &.close{
        opacity: 0
      }      
    }   

    .icon{
      padding-right: 10px;
    }

    .content{
      flex: 1 1 auto;
      text-align: center;
      font-weight: bold;
      padding: 15px 0;

      @include desktop-and-up {  
        text-align: left;
      }          

      &.content-padding{
        padding: 10px 0;
      }
    }

    .close-btn{
      padding: 10px;
      cursor: pointer;
    }

  }

</style>
