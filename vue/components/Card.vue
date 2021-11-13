<template>
    <div class=" p-5">
        <section class="max-w-sm rounded overflow-hidden shadow-xl ">
            <a class="hover-card" :href="'pokemon/'+name">
                <div class="px-6 py-4 bg-gray-200 not-hover">
                    <img class=" rounded-xl" :src="sprite">
                    <div class="mt-2 mb-2 flex justify-between">
                    <!-- <pre>
                        {{data}}
                    </pre> -->
                    <!-- <pre>
                        {{data.types[0].type}}
                    </pre> -->
                        <p class="font-bold pt-1 text-gray-800 text-xl">{{name | capitalize | gender}}</p>
                        <p class="font-bold pt-1 text-gray-400 text-lg">{{id | format_number}}</p>

                    </div>

                    <div class="flex mt-1">

                        <p class="text-gray-50 p-1 px-3 mr-3 rounded-3xl" :class="types[0]"> {{types[0] | capitalize}}</p>
                        <p v-if="types[1]" class="text-gray-50 p-1 p-1 px-3 rounded-3xl" :class="types[1]" >{{types[1] | capitalize}}</p>

                    </div>
                </div>

            </a>
        </section>
    </div>
</template>

<script>
export default {
    props:{
        id: {type:Number, required:true},
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
                        .replace('-f', ' ♀')
                        .replace('-m', ' ♂')
        }
    },
    head(){
        return{
            title: this.title,
            link: [{ rel: 'icon', type: 'image/png', href: '/pokeball.png' }],
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
        name(){
            if (this.data)
                return this.data.name
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


<style scoped>
@layer components{

    .grass   { @apply bg-green-500 }   
    .fire    { @apply bg-red-500   }
    .water   { @apply bg-blue-500  }

    .ice     { @apply bg-blue-200   }
    .flying  { @apply bg-indigo-300 }
    .electric{ @apply bg-yellow-400 }
    
    .psychic { @apply bg-pink-400   }
    .fighting{ @apply bg-red-800    }
    .ghost   { @apply bg-purple-900 }

    .steel   { @apply bg-gray-400   }
    .fairy   { @apply bg-pink-300 }
    .dragon  { @apply bg-purple-600 }
    
    .poison  { background: #B020DD  }
    .normal  { background: #A0A090  }
    .bug     { background: #A0BA50  }

    .ground  { background: #deb25f  }
    .rock    { background: #bd9e60  }
    .dark    { background: #47352e  }

    .hover-card 
    
    :hover{
        @apply border-2;
        @apply border-red-500;
    }
    .not-hover :hover{
        @apply border-0;
        @apply border-blue-500;
    }

}
</style>