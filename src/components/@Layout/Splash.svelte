<script lang='ts'>
  import {getContext} from 'svelte';
  import {DeviceStore} from '@store/store'
  import Button from '@base/Button.svelte'

  export let title = 'Splash Title';
  export let version = null;
  export let buttonOne = null;
  export let buttonTwo = null;

  const {isTabletAndBelow} = DeviceStore;

  const theme:string = getContext('theme');

</script>

<div class={`splash ${theme}-theme`}>
  <h5 class='title'>
    {title}
  </h5>  
  {#if !!version}
    <h5 class='version'>
      v{version}
    </h5>
  {/if}
  <div class='buttons'>
    {#if buttonOne}
      <Button style={'min-width: 100px'} size={$isTabletAndBelow ? 'small' : null} {...buttonOne} />
    {/if}
    {#if buttonTwo}
      <Button style={'min-width: 100px'} size={$isTabletAndBelow ? 'small' : null}  {...buttonTwo} />
    {/if}      
  </div>
</div>

<style lang='scss'>
  @import "../../scss/src/_media-queries.scss";

  .splash{
    width: 100%;
    padding: 10px 0;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;    
    background: transparent;
    color: var(--white-0-text);

    @include desktop-and-up {
      padding: 50px 0;
    }    

    &.dark-theme{
      color: var(--black-0-text);
    }    

    .title{
      font-size: 32px;
      @include desktop-and-up {
        font-size: 72px;
      }
      // margin-bottom: 10px;
    }

    .version{
      margin-bottom: 5px;
      @include desktop-and-up {
        margin-bottom: 10px;
      }      
    }

    .buttons{
      display: flex;
      gap: 10px;    
    }    

  }
</style>

