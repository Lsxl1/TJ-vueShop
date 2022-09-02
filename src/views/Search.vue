<template lang="html">
  <body>
      <div>
        <!-- 圆边搜索框 -->
          <form action="/">
          <van-search
          v-model="searchval"
          shape="round"
          background=white
          show-action
          placeholder="请输入商品名称"
          @search="onSearch"
          @cancel="onCancel"/>
          <!-- input表示输入框发生变化 -->
        </form>

        <!-- 调用商品内容组件 -->
        <SearchProducts 
        v-if="blockshow==1"/>
      </div>
  </body>
</template>

<script>
import SearchProducts from "@/components/search/SearchProducts"
import { Toast } from 'vant';

export default {
  data() {
    return {
      //搜索的文字，由用户输入
      searchval:"",
      blockshow:0,
    };
  },

  methods: {
    onClickLeft(){
        this.$router.push({name:'分类页'})
    },
    onSearch(val) {
      if (this.type == 1) {
        this.get_serachProductbyName(val);
        console.log("1111111111111111111111111111111111111")
      } else if (this.type == 2) {
        this.get_serachProductbyType(val);
        console.log("2222222222222222222222222222222222222")
     }
    },
    onCancel() {
      // 取消搜索请求
      Toast('取消');
    },
    //获取搜索的商品
    get_serachProductbyName(val) {
      this.$net({
        method: "get",
        url: "/ShopTransaction/search_productInfo",
        params: {
          product_name: val,
        },
      })
        .then((response) => {
          this.$store.commit("changemessage", response.data);
          this.blockshow = 2;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    get_serachProductbyType(val) {
      this.$net({
        method: "get",
        url: "/ShopTransaction/search_product_type",
        params: {
          product_type: val,
        },
      })
        .then((response) => {
          this.$store.commit("changemessage", response.data);
/*           console.log(val+"!!!!!!!!!!!!!!")
 */          this.blockshow = 2;
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  components:{
    SearchProducts
  },
  mounted(){
     
  }
};
</script>

<style lang="less" scoped>
body{
  background-color: #eee;
}
</style>
