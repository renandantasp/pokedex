<template>
    <div class="table">
        <p class="font-black text-2xl p-5"> Base Stats </p>
            <table>
                    <tr v-for="(stat, id) in stats_val" :key="id">
                        <td class="name"> {{stats_name[id] | filter_name}} </td>
                        <td> {{stat}} </td>
                        <td><Bar :val="stat_percent(false,stat)"/> </td>
                    </tr>
                    <tr>
                        <td class="name"> Total </td>
                        <td class="font-bold"> {{sum_stat()}} </td>
                        <td><Bar :val="stat_percent(true)" /></td>
                    </tr>
            </table> 
    </div>
</template>

<script>
import Bar from '@/components/Bar'
export default {
    components:{
        Bar
    },
    props:{
        id:{type:String, required:true}
    },
    filters:{
        filter_name: function(value){
            if (!value) return ''
            value = value.toString()
            return value.replace('_', ' ')

        }
    },
    data(){
        return{
            data:{},
            stats_name:['HP','Attack','Defense','Special_Attack', 'Special_Defense', 'Speed'],
            stats_val:[],
        }
    },
    methods:{
        sum_stat(){
            let sum = 0
            for(let i=0; i<6;i++){
                sum += this.stats_val[i]
            }
            return sum
        },
        stat_percent(total=false, val=0){
            let sum = 0
            if (total == true){
                for(let i=0; i<6;i++){
                    sum += this.stats_val[i]
                }
                return Math.round((sum/1530)*100)
            }
            return  Math.round((val/255)*100)
        }
    },
    async created(){
        const response = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/${this.id}`)
        this.data = response
        let stats = new Array(6)
        let sum
        for(let i=0;i<6;i++){
            stats[i] = response.stats[i].base_stat
        }
        this.stats_val = stats
    }
}
</script>

<style lang="scss" scoped>
@layer components{
    table{
        table-layout: fixed;
        @apply mb-3;
        tr{
            border-bottom: solid 1px #d1d5db;
            @apply mx-4;
        }
        td{
            text-align:center;
            width: 80px;
            @apply px-2;
        }

    }
    .table{
        text-align: center;
        border-radius: 5px; 

        .name{
            text-align: end;
            color: #808080;
            width: 120px;
        }
    }
}
</style>