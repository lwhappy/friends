<template>
  <div class="index-container">
    <h1><strong>胖番茄网</strong> -- <em>老友记台词训练</em></h1>
    <div v-for="outerItem in seasons">
      <h3>第{{outerItem}}季</h3>
      <ul>
        <li v-for="innerItem in episode" @click="go(outerItem,innerItem)">第{{innerItem}}集</li>
      </ul>
    </div>
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
        
      seasons:10,
      episode:24


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
    go : function(season,episode){
      var that = this;
      that.$router.push({ name: 'show', params: { season:season,episode:episode}})
      
    },
  }
}
</script>

<style lang="less" scoped>
  .index-container{
    padding:10px;
  }
  ul{
    overflow:hidden;
  }
  ul li{
    float:left;
    margin:10px;
    list-style:none;
  }
  h1{
    font-size:18px;
  }
  h3{
    font-size:16px;
  }
  em{
    font-style:normal;
  }
</style>
