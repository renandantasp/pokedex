<template>
    <div class=" p-5">
        <section class="card max-w-sm rounded overflow-hidden shadow-xl">
            <a :href="'pokemon/'+name">
            
                <div class="px-7 py-5 ">
                    <div>

                        <img :src="sprite">
                        
                        <div class="mt-2 mb-2 flex justify-between ">
                            <p class="font-bold pt-1 name text-xl">{{name | capitalize | gender}}</p>
                            <p class="font-bold pt-1 number text-lg">{{number | format_number}}</p>
                        </div>

                        <div class="flex mt-1">
                            <p class="text-gray-50 p-1 px-3 mr-3 rounded-3xl" :class="types[0]"> {{types[0] | capitalize}}</p>
                            <p v-if="types[1]" class="text-gray-50 p-1 p-1 px-3 rounded-3xl" :class="types[1]" >{{types[1] | capitalize}}</p>
                        </div>
                    </div>
                </div>

            </a>
        </section>
    </div>
</template>

<script>
export default {
    props:{
        id: '',
    },
    data(){
        return{
            data:{},
            title:'Pokedex',
        }
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
            return '#'+value
        },
        gender: function(value){
            if (!value) return ''
            return value.toString()
                        .replace('an-f', 'an ♀')
                        .replace('an-m', 'an ♂')
        }
    },
    
    async created(){
        try {
            this.data = await this.$axios.$get(`https://pokeapi.co/api/v2/pokemon/${this.id}`)
        } catch(e) {
            console.error(e.message)
        }
    },
    computed:{
        number(){
            return this.data.id
        },
        name(){
            if (this.data) return this.data.name
            return ''
        },
        sprite(){
                return this.data.sprites?.other['official-artwork']?.front_default || ''
        },
        types(){
                return this.data.types?.map( (entry) => entry.type.name ) || [0,0,0,0,0,0]
        }
    }
}
</script>


<style  lang="scss" scoped>
.card{
    border-radius: 20px;
    background: #504945;
    border-width: 2px;
    border-color: #554d49;
    &:hover{
        background: #f9f5d7;
        .name{
            color:#504945;
        }
        .color{
            color:#7c6f64;
        }
        border-color: #f9f5d7;
        transition: 0.5s;
    }
    .name{
        font-family: 'M PLUS Rounded 1c', sans-serif;
        font-weight: 800;
        transition: 0.5s;
        color: #f9f5d7;
    }
    .number{
        font-family: 'M PLUS Rounded 1c', sans-serif;
        transition: 0.5s;
        color: #928374;
    }
}

.not-hover{
    pointer-events:none;
}

</style>