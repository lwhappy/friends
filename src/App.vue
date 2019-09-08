<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style lang="less">
@import '~vux/src/styles/reset.less';

body {
  background-color: #fbf9fe;
}
#app{
	font-family:"Helvetica Neue",Helvetica,Arial,"Hiragino Sans GB","WenQuanYi Micro Hei","Microsoft YaHei",sans-serif;
}
</style>
