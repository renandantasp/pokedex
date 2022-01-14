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
      title: this.title,
      link: [
        { rel: 'icon', type: 'image/png', href: '/pokeball.png' },
        {rel: 'stylesheet', href:"https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css"}
      ],
  }
},
}
</script>
