<template>
<div>

    <!-- Previous and Next Pokémon  -->
    <div class="arrows">
        <a class="left" :href="previous[0]">
            <img src="@/static/right-arrow.png" style="transform: scaleX(-1); padding-top:5px" width="10" >
            {{previous[0] | capitalize | gender}}  {{previous[1]| format_number}}
            </a>
        <a class="right" :href="next[0]">
            {{next[0] | capitalize | gender}} {{next[1] | format_number}} 
            <img src="@/static/right-arrow.png" style="padding-top:5px" width="10">
            </a>
    </div>

    <!-- Page Components -->
    <div class="page">
        <div class="page-item img">
            <img :src="sprite">
        </div>
        <PageInfo 
            :name="id" 
            :types="types" 
            :info="info" 
            :abilities="abilities" 
            class="page-item"
        />

        <div class="break"></div>

        <PageStats :stats="stats" class="page-item"/>
        <PageType  :type_relation="type_relation" class="page-item  "/>

        <div class="break"></div>

        <p>evolution</p>
    </div>
</div>
</template>

<script>
import PageInfo  from '@/components/PageInfo.vue'
import PageStats from '@/components/PageStats.vue'
import PageType  from '@/components/PageType.vue'

export default {
    layout: 'default',
    components:{
        PageInfo,
        PageStats,
        PageType
    },
    props:{
        id:{type:String, required:true},

    },
    data(){
        return{
            data:{},
            next:[],
            previous:[],
            type_relation:[]
        }
    },
    async created(){
        const response = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/${this.id}`)
        this.data = response
        
        const types = this.data.types?.map( (entry) => entry.type.name ) || []
        for (var t of types){
            const type_rel = await this.$axios.$get(`https://pokeapi.co/api/v2/type/${t}`)
            this.type_relation.push({name:t,rel:type_rel.damage_relations})
        }



        let res

        if (response.id+1 == 899)
            res = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/1`)
        else
            res = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/${response.id+1}`)

        this.next = [res.name, res.id]

        if (response.id-1 == 0)
            res = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/898`)
        else
            res = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/${response.id-1}`)

        this.previous = [res.name, res.id]

    },
    computed:{
        sprite(){
            return this.data.sprites?.other['official-artwork']?.front_default || ''
        },
        types(){
            return this.data.types?.map( (entry) => entry.type.name ) || []
        },
        stats(){
            return this.data.stats?.map( (entry) => entry.base_stat)
        },
        info(){
            return {'height':this.data.height,'weight':this.data.weight}
        },
        abilities(){
            return this.data.abilities?.map((entry) => (
                {name:entry.ability.name, is_hidden:entry.is_hidden}
            ))
        },
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
            return 'N°'+value

        },
        gender: function(value){
                if (!value) return ''
                return value.toString()
                            .replace('n-f', 'n ♀')
                            .replace('n-m', 'n ♂')
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
        image-rendering: pixelated;                 /* Chrome */
        -ms-interpolation-mode: nearest-neighbor;   /* IE8+                           */
    }
    .page{
        display: flex;
        margin-left:5vw;
        margin-right:5vw;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        align-items: stretch;
        align-content: center;
        // border: solid 1px black;

        .break {
            flex-basis: 100%;
            height: 0;
        }
        .page-item{
            height:500px;
            width: 30vw;
            min-width:450px;
            // border-radius: 8px;
            // border: solid 1px #cccccc;
            margin: 10px;
        }
        .img{
            display:flex;
            justify-content:center;
            align-items:center;
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
            background: #504945;
            color: #d5c4a1;
            @apply font-bold;
            border-radius: 0 0px 0px 10px;
            border-right: solid #303030;
            padding:8px;

        }
        .right{
            background: #504945;
            color: #d5c4a1;
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