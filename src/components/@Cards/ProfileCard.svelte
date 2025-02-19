<script lang='ts'>
  import {getContext} from 'svelte';
  import FullImage from '@images/FullImage.svelte';
  import LoremBlock from '@base/LoremBlock.svelte'
  import InnerContainer from '@base/InnerContainer.svelte'
  import SVG from '@base/SVG.svelte'
  import Button from '@base/Button.svelte'


  export let shadow = false;
  export let innerShadow = false;
  export let rounded = false;
  export let useGradiant = false;
  
  export let orientation = 'vertical'
  export let type = 'black'
  export let imageType = 'rounded'
  export let applyTheme = 'primary';

  export let name = 'name'
  export let title = 'title'
  export let profession = 'profession'  
  export let imageSrc = null; 
  export let bgSrc = null;
  export let links = []

  export let onNameClick = null;
  export let onProfessionClick = null;
  export let onTitleClick = null;
  export let onImageClick = null;
  

  const theme:string = getContext('theme')

  let cardWidth = null;
  let contentHeight = null;
  let height = null;  

  const setHeight = () => {
    if(height === null){
      height = contentHeight;
    }
  }

  $: cardSize = () => {
    if( cardWidth >= 720 ){
      return 'large'
    }
    if( cardWidth < 720 && cardWidth > 640 ){
      return 'medium'
    }
    if( cardWidth < 640 && cardWidth > 420  ){
      return 'small'
    }
    if( cardWidth < 420  ){
      return 'tiny'
    }    
  }

  $: iconSize = () => {
    if( cardWidth < 720  ){
      return 18
    }    
    else{
      return 14
    }
  }  
  

  $ : {
    contentHeight !== null && setHeight()
  }
  
</script>

<div class={`profile-card ${type}-theme ${cardSize()} ${orientation} ${applyTheme}${useGradiant ? '-gradiant' : ''}`} class:rounded={rounded} class:shadow={shadow} bind:clientWidth={cardWidth}>
  
  <div class={`profile ${imageType}`} class:innerShadow={innerShadow}>
    <div class={`inner ${imageType}`}>

      {#if !!profession && profession?.length > 0}
        <div class='professionplate' class:clickable={!!onNameClick} on:click={() => {onProfessionClick && onProfessionClick()}}>
          {profession}
        </div>
      {/if}
      
      {#if !!imageSrc}
        <div class={`image`} class:clickable={!!onImageClick} on:click={onImageClick && onImageClick()}>
          <FullImage src={imageSrc} alt={imageSrc} />
        </div>

        {#if !!bgSrc}
          <div class={`image-bg`}>
            <FullImage src={bgSrc} alt={bgSrc} />
          </div>        
        {/if}
      {/if}

      {#if !!title && title?.length > 0}
        <div class='titleplate' class:clickable={!!onNameClick} on:click={() => {onTitleClick && onTitleClick()}}>
          {title}
        </div>    
      {/if}
    </div>
  </div>


  <div class={`details`} class:innerShadow={innerShadow}>
    <div class='header' class:clickable={!!onNameClick} on:click={() => {onNameClick && onNameClick()}}>
      <h2>{name}</h2>
    </div>
    <div class='content' bind:clientHeight={contentHeight}>  
      {#if !!height}
        <InnerContainer skinnybar height={`${height}px`}>
          <slot name='content'>
            <LoremBlock ignoreTheme />
          </slot>
        </InnerContainer>
      {/if}
    </div>
    <div class='footer'>
      {#each links as {icon, href}}
        <Button {href} exactfit size='small' nobg >
          <SVG {icon} size={iconSize()} applyTheme={type === 'dark' ? cardSize() === 'tiny' ? 'white' : applyTheme : cardSize() === 'tiny' ? 'white' : 'black'} />
        </Button>
      {/each}
    </div>
  </div>  
</div>

<style lang='scss'>
  @import "../../scss/src/_media-queries.scss";

  .profile-card{
    width: 100%;
    height: auto;
    display: flex;
    flex-direction: row;    
    overflow: hidden;
    background: var(--white-0);
    color: var(--white-0-text);    

    .clickable{
      cursor: pointer
    }    

    .profile{      
      width: auto;
      height: 100%;     
      overflow: hidden; 
      
      &.innerShadow{
        box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.50);  
      }

      .inner{
        position: relative;
        height: calc(100% - 80px);
        padding: 40px;
        display: flex;   
        justify-content: center;
        align-items: center;
        flex-direction: column;
        gap: 20px;          
      }

      .professionplate, .titleplate{
        text-align: center;
        padding: 2px 10px;
        border-radius: 10px;
        z-index: 1;
      }      

      .image{        
        width: 100px;
        height: 100px;
        overflow: hidden;  
        z-index: 1;
      }

      .image-bg{
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;        
        filter: grayscale(100%);        
      }      

      &.background{
        padding: 0;

        .inner{
          justify-content: space-between;
        }

      }    

      &.rounded{
        .image{
          border-radius: 10px;
        }
      }        

      &.circle{
        .image{
          border-radius: 50%;
        }
      }      
    }


    .details{
      width: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      gap: 10px;

      .header, .footer{
        display: flex;
        width: calc(100% - 40px);
        padding: 10px 20px;
      }

      .content{
        width: calc(100% - 40px);
        padding: 0 20px;
      }

      .footer{
        justify-content: flex-end;
        align-items: center;
      }

      .content{
        flex: 1 1 auto;
      }
    }


    &.vertical{
      flex-direction: column;

      .header{
        justify-content: center;
      }

      .image{
        width: 200px;
        height: 200px;
      }      
    }
    

    &.tiny{
      flex-direction: column;

      .inner{
        position: relative;
        height: calc(100% - 40px);
        padding: 20px; 
      }      

      .header{
        justify-content: center;        
      }

      .profile{
        padding: 0;
        width: 100%;
        height: auto;
      }

      .image{
        width: 100px!important;
        height: 100px!important;
      }

      .details{
        height: 300px;

        &.innerShadow{
          .footer{
            box-shadow: 5px -5px 15px rgba(0, 0, 0, 0.50);  
          }
        }
      }  
    }


    &.rounded{
      border-radius: 10px;
    }

    &.shadow{
      box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.50);  
    }

    &.dark-theme{
      background: var(--black-1);
      color: var(--black-1-text);
    }    


    $themes: 'primary', 'secondary', 'magic', 'success', 'warning', 'danger';
    @each $theme in $themes {
      &.#{$theme}{
        
        .profile{
          background: var(--#{$theme}-5);
        } 

        .professionplate, .titleplate{
          background: var(--#{$theme}-7);
          color: var(--#{$theme}-7-text);
        }

        .image{
          background: var(--#{$theme}-8);          
        }

        &.tiny{
          .footer{
            justify-content: space-around;
            background: var(--#{$theme}-5);
          }
        }        
      }
    }

    $themes: 'primary', 'secondary', 'magic', 'success', 'warning', 'danger';
    @each $theme in $themes {
      &.#{$theme}-gradiant{
        
        .profile{
          background: linear-gradient(var(--#{$theme}-5), var(--#{$theme}-6));
        } 

        .professionplate, .titleplate{
          background:  linear-gradient(var(--#{$theme}-7), var(--#{$theme}-8));
          color: var(--#{$theme}-7-text);
        }

        .image{
          background: var(--#{$theme}-8);
        }

        &.tiny{
          .footer{
            justify-content: space-around;
            background: linear-gradient(var(--#{$theme}-5), var(--#{$theme}-6));
          }
        }        
      }
    }

    
  }
</style>