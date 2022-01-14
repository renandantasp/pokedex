<template>
  <div> 
    <div align="center" class="pt-5">
      
      <input type="text" name="search-bar" @keyup="doSearch" v-model="query">
      <!-- <div class="resultado" v-if="listResult">
        <ul>
          <li v-for="pkmn in listResult" :key="pkmn.refIndex">
            <b>{{pkmn.item}}</b>
          </li>
        </ul>
      </div> -->
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