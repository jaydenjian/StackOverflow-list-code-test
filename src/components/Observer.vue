<template>
<div class="observer"></div>
</template>

<script>
export default {
    name: "observer",
    data(){
        return{
            observer: null,
        };        
    },

    mounted(){
        this.observer = new IntersectionObserver(([entry])=>{
            if(entry && entry.isIntersecting){
                console.log("intersect!");
                this.$emit('intersect'); // emit the intersect event at App.vue
            }
        });

        this.observer.observe(this.$el);
    }

}
</script>

<style scoped>

</style>