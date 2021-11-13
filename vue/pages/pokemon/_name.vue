<template>
<Page :id="name"/>
</template>

<script>
import Page from '@/components/Page'
export default {
  layout: 'default',
  components:{
    Page
  },
  data(){
    return{
      name:this.$route.params.name,
      data:{},
      name_caps: 'Pokédex',
      front_default: ''
    }
  },
  filters: {
    capitalize: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
    },
    format_number: function (value) {
        if (!value) return ''
        value = value.toString()
        while (value.length < 3){
            value = '0' + value
        }
        return '#'+value
    },
    gender: function(value){
            if (!value) return ''
            value = value.toString()
            value = value.replace('-f', ' ♀')
            value = value.replace('-m', ' ♂')
            return value
    }
  },
  head(){
        return{
            title: this.name_caps,
            link: [{ rel: 'icon', type: 'image/png', href: this.front_default }],
        }
    },
  async created(){
    const response = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/${this.name}`)
    this.data = response
    this.name_caps = this.data.name.charAt(0).toUpperCase() + this.data.name.slice(1)
    this.front_default = this.data.sprites.other['official-artwork'].front_default
  },
 
}
</script>
