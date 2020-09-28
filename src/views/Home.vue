<template>
  <div>
    <div class="top">
      <van-search show-action label="郑州" placeholder="请输入搜索关键词">
        <template #action>
          <div>搜索</div>
        </template>
      </van-search>
    </div>
    <!-- 导航 -->
    <div class="Nav">
      <div v-for="(item,index) in Nav" :key="index" @click="cd(item)" class="nav_box">
        <img :src="item.img" alt />
        <br />
        {{item.title}}
      </div>
    </div>
    <p></p>
    <!-- 列表数据 -->
    <div class="entry">
      <h4>猜你喜欢</h4>
        <van-card
          v-for="(item,index) in entry"
          :key="index"
          :num="item.monthSalesTip"
          :price="item.averagePriceTip+'元'"
          :desc="item.shipping_time"
          :title="item.shopName"
          :thumb="item.picUrl"
          @click="skip('/about',item.mtWmPoiId)"
        >
        <template #num>
          <span>{{item.monthSalesTip}}</span>
        </template>

        </van-card>  
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      Nav: [],
      entry: [],
    };
  },
  mounted() {
    this.$axios.get("http://localhost:8080/data.json").then((res) => {
      // console.log(res.data.gongge);
      this.Nav = res.data.gongge;
    });
    this.$axios.get("http://localhost:8080/listData.json").then((res) => {
      // console.log(res.data.data.shopList);
      this.entry = res.data.data.shopList;
    });
  },
  methods:{
    // 跳转
    skip(path,item){
      this.$router.push({
        path,
        query:{
          item
        }
      })
    },
    cd(item){
      this.$router.push({
            path:'/item',
            query:{
              item
            }
      })

    }
  }
};
</script>

<style scoped>
.top{
  position: fixed;
  height: 50px;
  width: 100%;
}
.Nav {
  padding-top: 60px;
  width: 100%;
  height: 175px;
  /* border: 1px solid red; */
  display: flex;
  flex-wrap: wrap;
  /* align-items: center; */
  text-align: center;
}
.nav_box {
  width: 80px;
  height: 100px;
}
p {
  width: 100%;
  height: 10px;
  background: rgb(240, 239, 247);
}
.entry {
  width: 100%;
  /* border: 1px solid red; */
}
</style>
