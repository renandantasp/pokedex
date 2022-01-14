<template>
    <div class="bar">
        <span :style="'width:'+val+'%;'" :class="stat_class"></span>
    </div>
</template>

<script>
export default {
    props:{
        val:0,
        stat:'',
        total:''
        
    },
    data(){
        return{
            stat_class:'',
        }
    },
    created(){
        if (this.val < 26){
            this.stat_class = 'low';
        }else if (this.val < 35){
            this.stat_class = 'medium';
        }else if (this.val < 62){
            this.stat_class = 'high';
        }
        else{
            this.stat_class = 'super';
        }
    }
}
</script>


<style lang="scss" scoped>
@layer components{
    @mixin stat_color($color, $secondary_color, $border_color){
         @apply border-2;
         background: repeating-linear-gradient(
             90deg,
             $color,
             $color 10px,
             $secondary_color 10px,
             $secondary_color 20px
         );
         border-color: $border_color;
        background-size: 160px 160px;
    }

    .bar{
        box-sizing: content-box;
        height: 30px;
        width:500px;
        position: relative;
        // border-right: solid 1px #ccc;
        margin: 5px 0 5px 0;
        color:transparent;
        border-radius: 0;
        padding: 5px;

        span{
            display: block;
            height: 60%;
            transform: translateY(20%);
            border-radius: 5px;
            animation: grow 1s , grad 1s linear infinite;
        }
    }
    @keyframes grow { 
        0% {width:0px} 
    }
    @keyframes grad { 
        0%{background-position: 0px}
        100%{background-position:   20px}
    }
    .low{
        @include stat_color(#ab413c, #82303a,#82303a)
    }
    .medium{
        @include stat_color(#d79921, #c27d15,#c27d15)
    }
    .high{
        @include stat_color(#689d6a, #528764,#528764)
    }
    .super{
        @include stat_color(#458588, #427385,#427385)
    }
}
</style>