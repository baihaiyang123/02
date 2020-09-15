<template>
  <div>
      <img :src="list.img" alt="">
      {{list.title}}
  </div>
</template>

<script>
export default {
    data(){
        return{
            list:[]
        }
    },
    mounted(){
        this.list=this.$route.query.item
    }
}
</script>

<style>

</style>