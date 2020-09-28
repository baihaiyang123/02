<template>
  <div>
      <img :src="list.img" alt="">
      {{list.title}}
      <p>111</p>  </div>
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