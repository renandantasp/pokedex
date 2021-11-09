<template>
    <div class="bg-gray-200 border-2 border-gray-200 mb-3 mx-4">
            <table v-for="(stat, id) in stats_val" v-bind:key="id">
                <tr>
                    <td >{{stats_name[id] | filter_name}}</td>
                    <td>
                        <Bar :val="stat" :stat="stats_name[id]" />
                    </td>
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
    async created(){
        const response = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/${this.id}`)
        this.data = response
        let stats = new Array(6)
        for(let i=0;i<6;i++){
            // this.stat_name[i] = response.stats[i].stat.name
            stats[i] = response.stats[i].base_stat
        }
        this.stats_val = stats
    }
}
</script>

<style lang="scss" scoped>
.nsei{
    text-align: end;
}
</style>