<template>
    <div v-if="dmg.length">
        <slot></slot>
        <div class="display">
            <div v-for="tp in dmg" :key="tp">
                <p :class="tp">{{tp | capitalize}}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props:{
        dmg:[]
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
    flex-wrap: wrap;
    margin-left: 3vw;
    margin-right: 3vw;
    padding:5px;
    div{
        padding:4px;
    }
    flex-direction: row;
    justify-content: center;
}
.center{
    text-align:center;
}
</style>