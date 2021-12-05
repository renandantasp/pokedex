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