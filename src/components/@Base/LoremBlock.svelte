<script lang='ts'>
  import {getContext} from 'svelte';
  export let style = null;
  export let index = null;
  export let ignoreTheme = false;
  export let nopadding = false;

  export let content = null;
  export let onClick = null;
  export let length = 1;

  let loremText = "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Nulla at volutpat diam ut venenatis tellus in metus. Molestie nunc non blandit massa enim nec dui. Orci ac auctor augue mauris augue neque gravida in. Egestas maecenas pharetra convallis posuere morbi leo urna molestie at. A diam maecenas sed enim ut sem. Scelerisque in dictum non consectetur a erat nam. Cursus eget nunc scelerisque viverra mauris. Id velit ut tortor pretium viverra suspendisse potenti nullam ac. Mauris rhoncus aenean vel elit scelerisque mauris. Mi sit amet mauris commodo quis imperdiet massa tincidunt. Penatibus et magnis dis parturient montes nascetur ridiculus mus mauris. Tellus integer feugiat scelerisque varius morbi enim nunc."

  $: arr = new Array(length)
  const theme:string = getContext('theme')
</script>

<div class={`lorem-blocks ${theme}-theme`}  class:ignoreTheme={ignoreTheme} class:nopadding={nopadding}>
  {#each arr as _a, index}
    <div class='lorem-block' {style} on:click={() => {onClick && onClick({index})}}>
        <p>
          {content || loremText}
        </p>    
    </div>
  {/each}
</div>

<style lang='scss' scoped>
  .lorem-blocks{
    padding: 10px;
    background: var(--white-0);
    color: var(--white-0-text);

    &.nopadding{
      padding: 0;
    }

    &.dark-theme{
      background: var(--black-3);
      color: var(--black-3-text);
    }
    
    &.ignoreTheme{
      background: inherit!important;
      color: inherit!important;
    }

  }

  .lorem-block{
    :not(:last-child){
      margin-bottom: 10px;
    }
  }
</style>