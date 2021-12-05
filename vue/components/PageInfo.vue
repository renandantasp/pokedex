<template>
    <div>
        <div class="table">

            <h1 class="text-4xl font-bold">{{name | capitalize | gender}}</h1>
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
                    <td class="ability">
                        <p v-for="ab in abilities" :key="ab.name"> {{ab.name | capitalize}} <span v-if="ab.is_hidden">(hidden ability)</span></p>
                    </td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    props:{
        name:'',
        types:'',
        abilities:'',
        info:''
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
        h1   { @apply my-4 }
        span { @apply font-medium }


        table{
            table-layout: fixed;
            width:100%;
            height:100%;
            tr{ border-bottom: solid 1px #d1d5db; }
            td{
                width: 80px;
                padding:10px;
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
                span{
                    @apply text-gray-600;
                }
            }
        }
}

</style>