<template>
  <div class="page-home">
    <div class="form">

      <van-field clickable readonly v-model="companies" is-link name="picker" @click="companiesShowPicker = true" />
      <van-popup v-model:show="companiesShowPicker" position="bottom">
        <van-picker show-toolbar :columns="companiesArray" @confirm="companiesOnConfirm"
          @cancel="companiesShowPicker = false" />
      </van-popup>
      <van-field clickable readonly v-model="category" is-link name="picker" @click="categoryShowPicker = true" />
      <van-popup v-model:show="categoryShowPicker" position="bottom">
        <van-picker show-toolbar :columns="categoryArr" @confirm="categoryOnConfirm"
          @cancel="categoryShowPicker = false" />
      </van-popup>
      <van-field v-model="keywords" :placeholder="placeholders[categoryIndex]" />
      <div style="margin: 16px;">
        <van-button round block type="info" @click="search">提交</van-button>
      </div>


      <!-- <div class="search">
        <div class="icon iconfont icon-sousuo"></div>
        <input class="uni-input" confirm-type="search" v-model="keywords" :placeholder="placeholders[category]" />
        <div class="btn-search" @click="search">确定</div>
      </div>  -->
    </div>
  </div>
</template>

<script>
import dateformat from 'dateformat'
import Toast from "vant/lib/toast";
import "vant/lib/toast/style";

// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  data() {
    return {
      companiesArray: ['国网省公司', '国网总部', '蒙西电网'],
      categoryArr: [{
        value: 0,
        text: '查企业',
      }, {
        value: 1,
        text: '查产品',
      }, {
        value: 2,
        text: '查地区',
      }],
      placeholders: [
        '输入企业名称，例如宏乐集团',
        '输入产品名称',
        '输入省份或地区名'
      ],
      companies: '国网省公司',
      category: '查企业',
      categoryIndex: 0,
      keywords: '',
      companiesShowPicker: false,
      categoryShowPicker: false,
    }
  },
  components: {
    HelloWorld
  },
  methods: {
    companiesOnConfirm(value) {
      this.companies = value;
      this.companiesShowPicker = false;
    },
    categoryOnConfirm(value) {
      this.category = value.text;
      this.categoryIndex = value.value;
      this.categoryShowPicker = false;
    },
    search() {
      const enterpriseQuery = new this.$AV.Query("itemList")
      enterpriseQuery.equalTo("enterprise_parent", this.companies);
      enterpriseQuery.find().then((res) => {
        console.log(res);
      }).catch(err => {
        console.log(err);
      });
    }
  },
  created() {

  }
}
</script>

<style lang="scss">
.page-home {
  .avatar {
    width: 2rem;
    height: 2rem;
    @include gPlaceholder();
    margin: 0 auto;
  }
}
</style>
