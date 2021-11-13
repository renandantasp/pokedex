<template>
    <div>
        <div class="table">

            <h1 class="text-4xl font-bold">{{name | capitalize}}</h1>
            <table>
                <tr>
                    <td class="name">Type:</td>
                    <td class="data">
                        <span v-for="t in types" :key="t" :class="t">{{t | capitalize}} </span>
                    </td>
                </tr>
                <tr>
                    <td class="name">Height:</td>
                    <td class="data">{{info.height | height}}</td>
                </tr>
                <tr>
                    <td class="name">Weight:</td>
                    <td class="data">{{info.weight | weight}}</td>
                </tr>
                <tr>
                    <td class="name">Abilities:</td>
                    <td class="ability">asdasdasd</td>
                    <!-- <td class="data">{{abilities}}</td> -->
                </tr>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    props:{
        name:'',
        types:[],
        abilities:[],
        info:{}
    },
    filters:{
        capitalize : function(value){
            if (!value) return ''
            value = value.toString() 
            return  value.charAt(0).toUpperCase() + value.slice(1)
        },
        height : function(value){
            if (!value) return ''
            value = value/10
            return (value.toString() + ' m')
        },
        weight : function(value){
            if (!value) return ''
            value = value/10
            return (value.toString() + ' kg')
        },

    },
}
</script>

<style lang="scss" scoped>
    @layer components{
        @mixin type($color){
            @apply text-gray-50;
            @apply p-1;
            @apply px-3;
            @apply mr-3;
            @apply rounded-3xl;
            background: $color;
        }
        h1{
            @apply my-4;
        }
        p{
            @apply font-bold;
            @apply text-lg;
        
        }
        span{
            @apply font-medium;
        }

        table{
            table-layout: fixed;
            width:100%;
            height:100%;

            // @apply mb-3;
            tr{
                border-bottom: solid 1px #d1d5db;
                // @apply mx-4;
            }
            td{
                // text-align:center;
                width: 80px;
                padding:10px;
                // @apply px-2;
            }

        }
        .table{
            .name{
                text-align: end;
                color: #808080;
                width:15px;
            }
            .ability{
                @apply text-blue-500;
                text-decoration: underline;
            }
        }

        .grass   { @include type(#10b981) }   
        .fire    { @include type(#ef4444) }
        .water   { @include type(#3b82f6) }

        .ice     { @include type(#bfdbfe) }
        .flying  { @include type(#a5b4fc) }
        .electric{ @include type(#fbbf24) }
        
        .psychic { @include type(#f472b6) }
        .fighting{ @include type(#991b1b) }
        .ghost   { @include type(#4c1d95) }

        .steel   { @include type(#9ca3af) }
        .fairy   { @include type(#f9a8d4) }
        .dragon  { @include type(#7c3aed) }
        
        .poison  { @include type(#B020DD)  }
        .normal  { @include type(#A0A090)  }
        .bug     { @include type(#A0BA50)  }

        .ground  { @include type(#deb25f)  }
        .rock    { @include type(#bd9e60)  }
        .dark    { @include type(#47352e)  }
}

</style>