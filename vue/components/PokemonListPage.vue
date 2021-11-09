<template>
<div>
    <div class="p-5 grid grid-cols-1 sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 2xl:grid-cols-5 gap-3">
        <PokemonCard v-for="(n, index) in pageOffset" :key="index" :id="index+1"/>
    </div>
    <footer ref="infinitescrolltrigger">
      <div id="scroll-trigger" v-if="showLoader"></div>
      <div class="circle-loader"></div>
    </footer>
</div>
</template>

<script>
import PokemonCard from '@/components/PokemonCard'
export default {
  components:{
    PokemonCard
  },
  data(){
    return{
      currentPage:1,
      maxPerPage: 20,
      totalResults:898,
      showLoader:true
    }
  },
  computed:{
    pageCount(){
      return Math.ceil(this.totalResults/this.maxPerPage)
    },
    pageOffset(){
      return this.currentPage * this.maxPerPage;
    }
  },
  methods:{
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
    border: 10px solid #ef4444;
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
