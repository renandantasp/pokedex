<template>
  <div> 
    <div align="center" class="pt-5 input-icons">
      <i class="icon fa fa-search"></i>
      <input class="search-bar" type="text" @keyup="doSearch" v-model="query">
    </div>

  </div>
</template>


<script>
import Fuse from 'fuse.js';

export default {
  data(){
    return{
      pkmn_list: {},
      query: '',
    }
  },
  methods:{
    doSearch(){
      this.$emit('search',this.listResult.map(entry => entry.item))
    },
    onSearch(){
      if(this.listResult.length == 0){
       return false
      }
      return true
       
    }
  },
  computed: {
    listResult() {
      const fuseConfig = {
        threshold: 0.2,location: 0,
        distance: 50, maxPatternLength: 32,
        minMatchCharLength: 2,

      }
      const fuse = new Fuse(this.pkmn_list, fuseConfig)
      return Array.from(fuse.search(this.query))
 
    }
  },
  async created(){
    const response = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon?limit=1120`)
    this.pkmn_list = response.results.map((entry) => entry.name)

  }
}
</script>

<style lang="scss" scoped>
.icon{
  color:#928374;
  padding: 35px;
  min-width: 40px;
}
.input-icons i{
   position: absolute;
}
.search-bar{
  background: #504945;
  padding:10px;
  padding-left: 40px;
  padding-right: 40px;
  width:30vw;
  color:#ebdbb2;
  margin:20px;
  border-width: 2px;
  border-color: #504945;
  border-radius: 30px;
  &:hover{
    background: #3c3836;
    border-color: #7c6f64;
  }
  &:focus{
    outline-width: 0;
    background: #3c3836;
    border-color: #7c6f64;
  }
}

</style>