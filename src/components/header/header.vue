<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}} / {{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="supports">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img width="100%" height="100%" :src="seller.avatar">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="(item,index) in seller.supports">
                <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                <span class="text">{{seller.supports[index].description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="hideDetail">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
  import star from '../star/star.vue';

  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data(){
      return {
        detailShow: false
      }
    },
    methods: {
      showDetail(){
        this.detailShow = true;
      },
      hideDetail(){
        this.detailShow = false;
      }
    },
    created(){
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components: {
      'star': star
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl" //导入mixin

  .header
    position relative //这里的相对定位是为了让下面的绝对定位做参考
    overflow: hidden
    color: #ffffff
    background: rgba(7, 17, 27, 0.5)
    .content-wrapper
      padding: 24px 12px 18px 24px
      font-size: 0 //这里为0是为了去掉行内元素的间隙,这个可以继承,所以需要在子孙元素里面再单独设置font-size
      position: relative //这里的相对定位是为了让下面的绝对定位做参考
      .avatar
        display: inline-block
        vertical-align: top
        img
          border-radius: 2px
      .content
        display: inline-block
        margin-left: 16px
        .title
          margin: 2px 0 8px 0
          .brand
            display: inline-block
            vertical-align: top
            width: 30px
            height: 18px
            bg-image('brand') //这里使用了一个图片的mixin
            background-size: 30px 18px
            background-repeat: no-repeat
          .name
            margin-left: 6px
            font-size: 16px
            line-height: 18px
            font-weight: bold
        .description
          margin-bottom: 10px
          line-height: 12px
          font-size: 12px
        .supports
          .icon
            display: inline-block
            vertical-align: top
            width: 12px
            height: 12px
            margin-right: 4px
            background-size: 12px 12px
            background-repeat: no-repeat
            &.decrease //这里就是配合classMap进行图片切换的
              bg-image('decrease_1') //这是图片的mixin,在mixin.styl里面可以看到
            &.discount
              bg-image('discount_1')
            &.guarantee
              bg-image('guarantee_1')
            &.invoice
              bg-image('invoice_1')
            &.special
              bg-image('special_1')
          .text
            line-height: 12px
            font-size: 10px
      .support-count
        position: absolute
        right: 12px
        bottom: 14px
        padding: 0 8px
        height: 24px
        line-height: 24px
        border-radius: 14px
        background: rgba(0, 0, 0, 0.2)
        text-align: center
        .count
          vertical-align: top //考虑到行内元素的不一致的对齐,需要大量使用line-height和vertical-align
          font-size: 10px
        .icon-keyboard_arrow_right
          margin-left: 2px
          line-height: 24px
          font-size: 10px
    .bulletin-wrapper
      position: relative //做相对定位,给icon-keyboard_arrow_right绝对定位做参照
      height: 28px
      line-height: 28px
      padding: 0 22px 0 12px
      white-space: nowrap
      overflow: hidden
      text-overflow: ellipsis //这里是字数太长的省略处理
      background: rgba(7, 17, 27, 0.2)
      .bulletin-title
        display: inline-block
        vertical-align: top
        margin-top: 8px
        width: 22px
        height: 12px
        bg-image('bulletin')
        background-size: 22px
      .bulletin-text
        vertical-align: top
        margin: 0 4px
        font-size: 10px
        font-weight: 200
      .icon-keyboard_arrow_right
        position: absolute
        font-size: 10px
        right: 12px
        top: 8px
    .background //这里使用的是图片做背景,所以需要做下面这些处理
      position absolute //背景图片需要绝对定位
      top: 0 //设置位置
      left: 0
      width: 100% //设置大小
      height: 100%
      z-index: -1 //将背景图片设置里层
      filter: blur(10px) //背景图片虚化
    .detail
      position: fixed //弹出层,所以用fixed
      z-index: 100 //需要设置z-index保持一定的高度
      top: 0
      left: 0
      width: 100%
      height: 100%
      overflow: auto //加overflow hidden来把多余的背景阴影去掉
      opacity: 1 //这是动画设置
      background: rgba(7, 17, 27, 0.8)
      &.fade-enter-active, &.fade-leave-active
        transition: all 0.5s
      &.fade-enter, &.fade-leave-active
        opacity: 0
        background: rgba(7, 17, 27, 0)
      .detail-wrapper
        width: 100%
        min-height: 100% //因为不知道有多少内容,所以用min-height
        .detail-main
          margin-top: 64px
          padding-bottom: 64px //预留下面close按钮的位置,所以下内边距顶起
          .name
            line-height: 16px
            text-align: center
            font-size: 16px
            font-weight: 700
          .star-wrapper
            margin-top: 18px
            padding: 2px 0
            text-align center
          .title
            display: flex //使用flex布局等分详情页优惠界面
            width: 80%
            margin: 28px auto 24px auto
            .line
              flex: 1 //使用1的比例的flex块
              position: relative //这里使用相对布局加top对齐,可以也可以margin-bottom
              top: -6px
              border-bottom: 1px solid rgba(255, 255, 255, 0.2) //使用border生成线,因为使用的是div
            .text
              padding: 0 12px
              font-size: 14px
              font-weight 700
          .supports
            width: 80%
            margin: 0 auto
            .support-item
              padding: 0 12px
              margin-bottom: 12px
              font-size: 0
              &:last-child
                margin-bottom: 0
              .icon
                display: inline-block
                width: 16px
                height: 16px
                vertical-align: top
                margin-right: 6px
                background-size: 16px 16px
                background-repeat: no-repeat
                &.decrease
                  bg-image('decrease_2')
                &.discount
                  bg-image('discount_2')
                &.guarantee
                  bg-image('guarantee_2')
                &.invoice
                  bg-image('invoice_2')
                &.special
                  bg-image('special_2')
              .text
                line-height: 16px
                font-size: 12px
          .bulletin
            width: 80%
            margin: 0 auto
            .content
              padding: 0 12px
              line-height: 24px
              font-size: 12px
      .detail-close
        position: relative
        width: 32px
        height: 32px
        margin: -64px auto 0 auto //之前预留了位置,现在需要填充回去
        clear: both
        font-size: 32px
</style>

