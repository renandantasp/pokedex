<template>
    <div>  
        <h1 class="font-black text-2xl p-5 text-center" > Type Effectiveness </h1>
        <div v-if="dmg_from.qd.length">
            <p>Receives 4× damage from:</p>
            <div class="display">
                <div v-for="tp in dmg_from.qd" :key="tp">
                    <p :class="tp">{{tp | capitalize}}</p>
                </div>
            </div>
        </div>

        <div v-if="dmg_from.db.length">
            <p>Receives 2× damage from:</p>
            <div class="display">
                <div v-for="tp in dmg_from.db"  :key="tp">
                    <p :class="tp" >{{tp | capitalize}}</p>
                </div>
            </div>
        </div>

        <div v-if="dmg_from.db.length">
            <p>Receives ½× damage from:</p>
            <div class="display">
                <div v-for="tp in dmg_from.hf"  :key="tp">
                    <p :class="tp" >{{tp | capitalize}}</p>
                </div>
            </div> 
        </div>

        <div v-if="dmg_from.qt.length">
            <p>Receives ¼× damage from:</p>
            <div class="display">
                <div v-for="tp in dmg_from.qt"  :key="tp">
                    <p :class="tp" >{{tp | capitalize}}</p>
                </div>
            </div>
        </div>

        <div v-if="dmg_from.no.length">
            <p>Receives no damage from:</p>
            <div class="display">
                <div v-for="tp in dmg_from.no"  :key="tp">
                    <p :class="tp" >{{tp | capitalize}}</p>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
export default {
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
    // border: 1px solid black;
    display: flex;
    flex-direction: row;
    justify-content: center;

    padding:10px;
}
p{
    text-align:center;
}
</style>