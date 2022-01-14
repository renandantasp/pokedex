<template>
    <div>  
        <h1 class="font-black text-2xl p-5 text-center" > Type Effectiveness </h1>
        <PageTypeEffect :dmg="dmg_from.qd"> 
           <p> Receives 4× damage from: </p>
        </PageTypeEffect>
        
        <PageTypeEffect :dmg="dmg_from.db"> 
           <p> Receives 2× damage from: </p>
        </PageTypeEffect>

        <PageTypeEffect :dmg="dmg_from.hf"> 
           <p> Receives ½× damage from: </p>
        </PageTypeEffect>

        <PageTypeEffect :dmg="dmg_from.qt"> 
           <p> Receives ¼× damage from: </p>
        </PageTypeEffect>

        <PageTypeEffect :dmg="dmg_from.no"> 
           <p> Receives no damage from: </p>
        </PageTypeEffect>

    </div>
</template>

<script>
import PageTypeEffect from '@/components/PageTypeEffect.vue'
export default {
    components:{
        PageTypeEffect
    },
    props:{
        type_relation:[]
    },

    computed:{
        dmg_from(){
            var dmg_from = {qd:[], db:[], hf:[], qt:[], no:[]}
            let db1 = this.type_relation[0]?.rel.double_damage_from.map( (entry) => entry.name) || []
            let hf1 = this.type_relation[0]?.rel.half_damage_from.map  ( (entry) => entry.name) || []
            let no1 = this.type_relation[0]?.rel.no_damage_from.map    ( (entry) => entry.name) || []

            if(this.type_relation[1]?.name){

                let db2 = this.type_relation[1]?.rel.double_damage_from.map( (entry) => entry.name) || []
                let hf2 = this.type_relation[1]?.rel.half_damage_from.map  ( (entry) => entry.name) || []
                let no2 = this.type_relation[1]?.rel.no_damage_from.map    ( (entry) => entry.name) || []

                dmg_from.no = no1.concat(no2)
                dmg_from.qd = db1.filter(val => db2.includes(val) && !dmg_from.no.includes(val))

                dmg_from.db = db1.filter(val => !db2.includes(val) && !hf2.includes(val) && !dmg_from.no.includes(val))
                                    .concat(db2.filter(val=> !db1.includes(val) && !hf1.includes(val) && !dmg_from.no.includes(val)))
                                    
                dmg_from.hf = hf1.filter(val => !hf2.includes(val) && !db2.includes(val) && !dmg_from.no.includes(val))
                                    .concat(hf2.filter(val=> !hf1.includes(val) && !db1.includes(val) && !dmg_from.no.includes(val)))

                dmg_from.qt = hf1.filter(val => hf2.includes(val) && !dmg_from.no.includes(val))
                return dmg_from
            }
            dmg_from.db = db1
            dmg_from.hf = hf1
            dmg_from.no = no1
            return dmg_from
        }
    },
    filters:{
        capitalize: function (value) {
            if (!value) return ''
            value = value.toString()
            return value.charAt(0).toUpperCase() + value.slice(1)
        },
    }
}
</script>

<style lang="scss" scoped>

.display{
    
    display: flex;
    flex-direction: row;
    justify-content: center;

    padding:10px;
}
h1, p{
    color: #f9f5d7;
    text-align:center;
}
</style>