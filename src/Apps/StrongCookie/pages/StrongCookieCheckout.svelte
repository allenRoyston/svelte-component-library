<script>
  import Channels from '@base/Channels'
  import HashWatch from '@core/HashWatch'
  import { createChannel } from '@js/utility'

  import Checkout from '../components/Checkout'
  import ProductDetails from '../components/ProductDetails'
  import TwoSlot from '@base/TwoSlot.svelte'
   
  //--------------------------- CHANNEL
  const channel = createChannel({
    data: [
      {content: Checkout},
      {content: ProductDetails},
    ]  
  }) 

  const gotoChannel = (index) => {
    channel.current = index
  }  

  const onChange = ({params}) => {        
    gotoChannel(params?.product ? 1 : 0)    
  }  
  //---------------------------  

</script>

<TwoSlot showLeft showRight={channel.current === 1} >
  <h1>Checkout</h1>
  <div slot='right'>
    <button style="cursor: pointer" on:click={() => {window.history.back()}}>Back</button>
  </div>
</TwoSlot>
<hr>

<HashWatch {onChange} />
<Channels {...channel} />