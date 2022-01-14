<template>
    <div>
        <TheNavbar/>
        <nuxt/>
    </div>
</template>

<script>
import TheNavbar from '@/components/TheNavbar.vue'
import "@/assets/css/main.css"
export default {
    components:{
        TheNavbar
    }
}
</script>

<style lang="scss" >
body{
    background: #2f2d2c;
}
</style>