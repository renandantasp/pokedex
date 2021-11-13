<template>
<div class="table">
    <h1 class="font-black text-2xl p-5"> Base Stats </h1>
    <table>
            <tr v-for="(stat, id) in stats" :key="id">
                <td class="name"> {{stats_name[id] | underscore}} </td>
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
        stats:[]
    },
    filters:{
        underscore: function(value){
            if (!value) return ''
            return value.toString()
                        .replace('_', ' ')

        }
    },
    data(){
        return{
            stats_name:['HP','Attack','Defense','Special_Attack', 'Special_Defense', 'Speed'],
        }
    },
    methods:{
        sum_stat(){
            if(!this.stats) return []
            let sum = 0
            for(let i=0; i<6;i++){
                sum += this.stats[i]
            }
            return sum
        },
        stat_percent(total=false, val=0){
            if(!this.stats) return []
            let sum = 0
            if (total == true){
                for(let i=0; i<6;i++){
                    sum += this.stats[i]
                }
                return Math.round((sum/1530)*100)
            }
            return  Math.round((val/255)*100)
        }
    },
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

        .name{
            text-align: end;
            color: #808080;
            width: 120px;
        }
    }
}
</style>