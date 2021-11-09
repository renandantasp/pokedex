<template>
    <div class="bar">
        <span :style="'width:'+val_percent+';'" :class="stat">{{val}}</span>
    </div>
</template>

<script>
export default {
    props:{
        val:Number,
        val_percent:String,
        stat:String
    },
    created(){
        let val_percent = Math.round((this.val/255)*100)
        this.val_percent = val_percent.toString() +'%'
    }
}
</script>


<style lang="scss" scoped>
.bar{
    box-sizing: content-box;
    height: 30px;
    width:500px;
    position: relative;
    margin: 10px 0 10px 0;
    background: transparent;
    color:#fff;
    border-radius: 25px;
    padding: 10px;
    box-shadow: inset 0 -1px 1px #fff5;

    span{
        display: block;
        height: 100%;
        border-radius: 8px;
        box-shadow: inset 0  2px 9px #fff5,
                    inset 0 -2px 6px #0006;
        position: relative;
        overflow: hidden;
        
    }
}
@layer components{
    .HP{ 
        @apply bg-red-500;
    }
    .Attack{
        @apply bg-blue-400;
    }
    .Defense{
        @apply bg-yellow-500;
    }
    .Special_Attack{ 
        @apply bg-indigo-400; 
    }
    .Special_Defense{ 
        @apply bg-yellow-600; 
    }
    .Speed{ 
        @apply bg-purple-500; 
    }
}
</style>