<template>
<div>
    <div class="arrows">
        <a class="left" :href="previous.name">
            <img src="@/static/right-arrow.png" style="transform: scaleX(-1)" width="20" >
            {{previous.name | capitalize}}  {{previous.id| format_number(1)}}
            </a>
        <a class="right" :href="next.name">
            {{next.name | capitalize}} {{next.id | format_number(1)}} 
            <img src="@/static/right-arrow.png" width="20" >
            </a>
    </div>
    <div class="">
        <img class="page-item bg-blue-100 border-2 border-gray-200 mb-3 mx-4 antialiased" width="500" :src="sprite" />
        <PokemonPageInfo  class=" page-item border-2 border-gray-200 mb-3 mx-4"/>
        <PokemonPageStats :id="id" class="page-item"/>
        <PokemonPageType  class=" page-item border-2 border-gray-200 mb-3 mx-4"/>
    </div>
</div>
</template>

<script>
import PokemonPageInfo from '@/components/PokemonPageInfo.vue'
import PokemonPageStats from '@/components/PokemonPageStats.vue'
import PokemonPageType from '@/components/PokemonPageType.vue'
export default {
    layout: 'default',
    components:{
        PokemonPageInfo,
        PokemonPageStats,
        PokemonPageType
    },
    props:{
        id:{type:String, required:true}
    },
    data(){
        return{
            data:{},
            next:{},
            previous:{},
            sprite:String
        }
    },
    async created(){
        const response = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/${this.id}`)
        this.data = response
        this.sprite = response.sprites.front_default
        if (response.id+1 == 899){
            this.next =  await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/1`)
        }else{
            this.next =  await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/${response.id+1}`)
        }
        if (response.id-1 == 0){
            this.previous = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/898`)
        }else{
            this.previous = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/${response.id-1}`)
        }

    },
    filters: {
    capitalize: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
    },
    format_number: function (value, mode) {
        if (!value) return ''
        value = value.toString()
        while (value.length < 3){
            value = '0' + value
        }
        if (mode == 1){
            return 'N°'+value
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
}
</script>

<style lang="scss" scoped>
@layer components{
    .antialiased { 
        image-rendering: optimizeSpeed;             /* STOP SMOOTHING, GIVE ME SPEED  */
        image-rendering: -moz-crisp-edges;          /* Firefox                        */
        image-rendering: -o-crisp-edges;            /* Opera                          */
        image-rendering: -webkit-optimize-contrast; /* Chrome (and eventually Safari) */
        image-rendering: pixelated; /* Chrome */
        image-rendering: optimize-contrast;         /* CSS3 Proposed                  */
        -ms-interpolation-mode: nearest-neighbor;   /* IE8+                           */
    }
    .page{
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        align-items: flex-start;

        .page-item{
            margin: 10px;
        }

    }
    .arrows{
        display: flex;
        justify-content: center;
        margin-left:20%;
        margin-right:20%;
        align-items: center;
        align-content: space-around;
        margin-bottom: 20px;
        .left{
            @apply bg-gray-400;
            @apply text-white;
            @apply font-bold;
            border-radius: 0 0px 0px 10px;
            border-right: solid #303030;
            padding:8px;

        }
        .right{
            @apply bg-gray-400;
            @apply text-white;
            @apply font-bold;
            border-radius: 0 0px 10px 0px;
            padding:8px;

        }
        img{
            display: inline;
            filter: invert(100%)
        }
    }
}
</style>