<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
  import header from './components/header/header.vue';

  const ERR_OK = 0;

  export default {
    data(){
      return {
        seller: {}
      }
    },
    created(){
      this.$http.get('/api/seller').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.seller = response.data;
          console.log(this.seller)
        }
      })
    },
    components: {
      'v-header': header
    }
  }
</script>
//需要告诉ide和vue-loader下面的css样式使用stylus来构建的
<style lang="stylus" rel="stylesheet/stylus">
  @import './common/stylus/mixin.styl'

  .tab
    display: flex // 使用flex布局
    width: 100%
    height: 40px
    line-height: 40px
    border-1px(rgba(7, 17, 27, 0.1))
    .tab-item
      flex: 1
      text-align: center
      & > a //stylus语法&代表父级
        display: block
        text-decoration: none
        font-size: 14px
        color: rgb(77, 85, 93)
        &.active
          color: rgb(240, 20, 20)
</style>
