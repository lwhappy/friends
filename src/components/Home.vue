<template>
  <div class="index-container">
    <h1><strong>胖番茄网</strong> -- <em>英语四级单词训练</em></h1>
    
  </div>
</template>



<script>
import Vue from 'vue'

import { Loading,XButton, Confirm } from 'vux'  

export default {
  components: {
    
    Loading,
    XButton,
    Confirm
    
  },
  data () {
    return {
        



    }
  },
  created(){
    var that = this;
    that.$vux.loading.hide();
    if(navigator.userAgent.toLowerCase().match("windows") !== null){
      this.$vux.toast.show({
       text: '使用手机或平板浏览此网站效果更佳',
       type: "text",
       width: '600px',
       position: 'top',
       time: 20 * 1000
      })
    }
    
  },
  methods: {
   
  }
}
</script>
<style lang="less">

</style>
<style lang="less" scoped>
@import '~vux/src/styles/1px.less';
@import '~vux/src/styles/center.less';
@import '../css/common.css';

</style>
