<template>
  <div class="container box-v-start align-stretch">
    <p class="name">{{content[index].name}}:</p>
    <div @click="isShowEn=!isShowEn">
      <div class="cn">{{content[index].cn}}</div>
      <div class="en" >
        <p :style="{visibility: isShowEn?'visible':'hidden'}">{{content[index].en}}</p>
      </div>
    </div>
    <div class="result">
      
      <div class="box-center result-items wrap" :class="isMatch?'success':''">
        <x-button :type="index === wordIndex ? 'primary':'default'" v-for="(item,index) in content[index].resultArr" @click.native="selectTarget(index)">{{item}}</x-button>
      </div>
    </div>
    
    <div class="input box-center wrap">
      <x-button type="default" v-for="item in content[index].enNew" @click.native="selectWord(item)">{{item}}</x-button>
    </div>
    <div class="box-start">
      <x-button type="default" @click.native="prev">上一句</x-button>
      <x-button type="default" @click.native="next">下一句</x-button>
    </div>
    <div class="box-start">
      <p>跳到第&nbsp;</p>
      <input class="row" v-model="row" />
      <p>&nbsp;/&nbsp;{{content.length}}&nbsp;句</p>
      <x-button type="default"  @click.native="gotoRow">确定</x-button>
    </div>
  </div>
</template>



<script>
import Vue from 'vue'
import axios from 'axios';
//console.log("config",config)

import { XButton, Toast ,Icon, XInput,Loading,Confirm,TransferDomDirective as TransferDom, ConfirmPlugin } from 'vux'
Vue.use(ConfirmPlugin)

export default {
  directives: {
    TransferDom
  },
  components: {
    
    XButton,
    
    Toast,
   
    Icon ,
    Loading,
    XInput,
    Confirm 
  },
  data () {
    return {
      isShowEn:true,
      content:[[]],
      index:0,
      row:1,
      wordIndex:0,
      isMatch:false


    }
  },
  watch : {
    
  },
  filters : {
    
  },
  created(){
    var that = this;
    var season = that.$router.history.current.params.season;
    var episode = that.$router.history.current.params.episode;
    const api = 'static/text/season'+season+'/'+ episode +'.js';
    $.ajax({
        url: api,
        type: "GET",
        dataType: "jsonp", //指定服务器返回的数据类型
        jsonpCallback: "showData",
        success: function (data) {
           console.log(data)
           document.title = data.title.en
           that.content = data.content
           for(let i = 0; i < that.content.length; i++) {
              that.content[i].enNew = that.content[i].en.split(' ')
              that.content[i].enNew.sort(function() {
                  return (0.5-Math.random());
              });
              that.content[i].resultArr = Array(that.content[i].enNew.length)
              for(let j = 0; j < that.content[i].resultArr.length;j++) {
                that.$set(that.content[i].resultArr,j,'')
              }
           }

        }
    });
    
    
  },
  methods: {
      init(){
        this.isMatch = false
        this.isShowEn = true
        this.wordIndex = 0
      },
      selectWord(word){
        if(this.isMatch){
          return
        }
        console.log(this.content[this.index].resultArr)
        this.$set(this.content[this.index].resultArr,this.wordIndex,word)
        this.$set(this.content,this.index,this.content[this.index])
        if(this.wordIndex < this.content[this.index].resultArr.length-1 ){
          this.wordIndex++
        }
        if(this.content[this.index].resultArr.join(' ') === this.content[this.index].en){
          this.isMatch = true
          this.wordIndex = ''
          this.$vux.toast.show({
           text: 'good',
           type: 'success'
          })
        }
      },
      selectTarget(index){
        this.wordIndex = index
      },
      gotoRow(){
        if(this.row){
          this.index = Number(this.row)-1
          this.init()
        }
      },
      prev(){
        if(this.index > 0){
          this.index--
          this.row = this.index + 1
          this.init()
        }
      },
      next(){
        if(this.index < this.content.length-1){
          this.index++
          this.row = this.index + 1
          this.init()
        }
      }
  }
}
</script>
<style lang="less">
  html,body,#app{
    height:100%;
  }
  body{
    background-color:#fff;
  }

  
</style>
<style lang="less" scoped>
@import '~vux/src/styles/1px.less';
@import '~vux/src/styles/center.less';
@import '../css/common.css';
.name{
  font-weight:600;
}
.container{
  padding:10px;
}
.container .wrap{
  flex-wrap: wrap;
}
.container /deep/ .weui-btn{
  padding:0 5px;
  width:auto;
  min-width:50px;
  height:50px;
  margin:5px;
}
.input{
  
  margin-top:10px;
}
.row{
  border:solid 1px #999;
  width:50px;
  height:30px;
  line-height:30px;
  text-indent:5px;
  margin-left:5px;
}
.success /deep/ .weui-btn{
  color:#330aee;
}
</style>
