<template>
  <div>
    <van-row>
      <van-col span="8">
        <div
          v-for="(item,index) in left_nav"
          @click="left_dj(item.spuList,index)"
          :key="index"
          :class="['left',{'tj':color==index}]"
        >{{item.categoryName}}</div>
      </van-col>
      <van-col span="16" class="shuju_neirong">

        <van-card
          v-for="(item,index) in right_entry"
          :key="index"
          :tag="item.unit"
          :price="item.currentPrice"
          :title="item.spuName"
          :thumb="item.littleImageUrl"
          :origin-price="item.originPrice"
        >
        <template #footer>
            <van-stepper button-size="20" v-model="item.index"  theme="round"  disable-input
            @change="jia(item)"
             />
        </template>
        </van-card>
      </van-col>
    </van-row>


       
       <van-submit-bar :price="gather" button-text="提交订单" >
            <van-icon name="cart-o" :badge="MyCart.length" size="50px" @click="showPopup"/>
       </van-submit-bar>


       <van-popup v-model="show" position="bottom" :style="{ height: '70%' }" >
              <div>
                  <van-icon name="delete"  @click="del"/>
              </div>
           
              <van-card
                  v-for="(items,index) in MyCart"
                  :key="index"
                 :tag="items.unit"
                 :price="items.currentPrice*items.index"
                 :title="items.spuName"
                 :thumb="items.littleImageUrl"
                 :origin-price="items.originPrice"
               >
            <template #num>
               <van-stepper v-model="items.index" theme="round"  button-size="22" disable-input />
            </template>
        </van-card>
    </van-popup>



  </div>
</template>

<script>
export default {
  data() {
    return {
      left_nav: [],
      color: "",
      right_entry: [],
      show:false,
      MyCart:[],
      
      
    };
  },
  computed:{
      gather(){
          var zong=0
          this.MyCart.map(item=>{
              zong+=item.currentPrice*item.index*100
          })
          return zong
      }
  },
  mounted() {
    let wldn = this.$route.query.item;
    console.log(wldn);
    this.$axios.get("http://localhost:8080/shopdetails.json").then((res) => {
      console.log(res.data);
      let data = res.data;
      data.map((i) => {
        if (i.data.mtWmPoiId.includes(wldn)) {
          this.left_nav = i.data.categoryList;
          this.right_entry=i.data.categoryList[0].spuList
        }
      });
    });

    let MyCart=localStorage.MyCart
    if(MyCart){
        this.MyCart=JSON.parse(MyCart)
    }






  },
  methods: {
    left_dj(item, i) {
      console.log(item);
      this.color = i;
      this.right_entry = item;
    },
    showPopup() {
      this.show = true;
    },
    jia(item){
        var zhui=true
        this.MyCart.map(i=>{
           console.log(i)
           if(i.spuId == item.spuId){
               zhui= false
           }
        })
        if(zhui){
            this.MyCart.push(item)
        }
        this.save()
    },
    del(){
        this.MyCart=[]
        this.save()

    },
    save(){
        localStorage.MyCart=JSON.stringify(this.MyCart)
    }



  },
};
</script>

<style scoped>
.left {
  width: 100%;
  height: 55px;
  box-sizing: border-box;
  padding: 10px;
  display: flex;
  align-items: center;

  font-size: 14px;
}
.tj {
  background:cyan;
  border-left: 2px solid red;
}

</style>