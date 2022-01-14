<template>
<ListPage/>
</template>

<script>
import ListPage from '@/components/ListPage'
export default {
  layout: 'default',
  components:{
    ListPage
  },
  head(){
    return{
      title: 'DEX',
      link: [
        { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' },
        {rel: 'stylesheet', href:"https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css"}
      ],
  }
},
}
</script>
