<template>
<div class="margin">
    <SearchBar @search="doShit"/>
    <!-- {{this.search_result}} -->
    <div class="p-5 grid grid-cols-1 sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 2xl:grid-cols-4 gap-3">
        <Card v-show="search_result.length == 0" v-for="(n, index) in pageOffset" :key="index" :id="index+1"/>
        <Card v-show="search_result.length != 0" v-for="pkmn in search_result" :key="pkmn" :id="pkmn"/>
    </div>
    <footer v-if="search_result.length == 0" ref="infinitescrolltrigger">
      <div id="scroll-trigger" v-if="showLoader"></div>
      <div class="circle-loader"></div>
    </footer>
</div>
</template>

<script>
import Card from '@/components/Card'
import SearchBar from '@/components/SearchBar'
export default {
  components:{
    Card,
    SearchBar
  },
  data(){
    return{
      currentPage:1,
      maxPerPage: 20,
      totalResults:898,
      showLoader:true,
      search_result: []
    }
  },
  computed:{
    pageCount(){
      return Math.ceil(this.totalResults/this.maxPerPage)
    },
    pageOffset(){
      return this.currentPage * this.maxPerPage;
    },

  },
  methods:{
    doShit(event){
      this.search_result= event
    },
    scrollTrigger(){
      const observer = new IntersectionObserver((entries) =>{
        entries.forEach(entry =>{
          if (entry.intersectionRatio > 0 && this.currentPage < this.pageCount){
            this.showLoader = true
            setTimeout(() =>{
              this.currentPage += 1
              this.showLoader = false;
            }, 2000);
          }
        });
      });

      observer.observe(this.$refs.infinitescrolltrigger)
    }
  },
  mounted(){
    this.scrollTrigger()
  }
}
</script>

<style lang="scss" scoped>
.margin{
  margin-left:15vw;
  margin-right:15vw;
}
footer{
  position: relative;
  width: 100%;
  height: 100px;

  #scroll-trigger{
      height: 50px;
  }
  .circle-loader{
    position: absolute;
    top: 50%;
    left: 50%;
    width: 50px;
    height: 50px;
    border-radius: 50%;
    border: 10px solid #cc241d;
    border-top: 10px solid #ffffff00;
    animation: rot 1s linear infinite;
  }
}

@keyframes rot { 
  100% { 
    transform:rotate(360deg); 
  } 
}
</style>
