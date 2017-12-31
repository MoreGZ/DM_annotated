<template>
  <div id="app">
      <div class="user">
          <div class="img">
              <img src="./assets/new_logo.png">
              <a href="http://www.baidu.com">退出</a>
          </div>
          <p class="name">{{username}}</p>
      </div>
      <div class="body">
          <div class="title">标注系统</div>
          <com-navbar v-on:turn_left="turnLeft" v-on:turn_right="turnRight"></com-navbar>
          <div class= "main">
              <com-type v-show="currentType==0" v-bind:username="username"></com-type>
              <com-abbr v-show="currentType==1"></com-abbr>
          </div>
      </div>
  </div>
</template>

<script>
  import comType from './components/Type.vue'
  import comAbbr from './components/Abbr.vue'
  import comNavbar from './components/Navbar.vue'
  import $ from 'jquery'

  export default {
    name: 'app',
    data:function(){
      return {
        username:"",
        currentType:0,
      }
    },
    components: {
      comType,
      comAbbr,
      comNavbar
    },
    created:function(){
        // 定义$on接受事件
        // 获取username
        this.username = $(".userName").html();
    },
    methods:{
        turnRight:function(){
            this.currentType = 1;
        },
        turnLeft:function(){
            this.currentType = 0;
        }
    }
  }
</script>

<style scoped lang="less">
  @color1:#fff;
  @color2:#99CCCC;
  @color3:#3399CC;
  #app{
    overflow: auto;
    width:100%;
    height:100%;
    .user{
      position: absolute;
      top:20px;
      left:50px;
      z-index: 3;
      .img{
        overflow: hidden;
        position: relative;
        border-radius: 50%;
        border: 1px solid @color3;
        width: 50px;
        height:50px;
        float: left;
        img{
          width:100%;
          height:100%;
        }
        a{
          width: 100%;
          height: 100%;
          position: absolute;
          top: 0px;
          left: 0px;
          background-color: @color3;
          display: block;
          opacity: 0;
          line-height: 50px;
          text-align: center;
          color: white;
          font-weight: 800;
          font-size: 12px;
          text-decoration: none;
          transition: all 0.3s linear;
          cursor: pointer;
        }
        a:hover{
          opacity: 0.8;
        }
      }
      p{
        font-size: 16px;
        line-height: 40px;
        float: left;
        margin-left: 10px;
        margin-top: 7px;
        width: 70px;
        height: 40px;
        text-align: left;
        color: @color3;
      }
    }
    .body{
      overflow: visible;
      background-color: #FFFFFF;
      padding: 10px 0;
      width: 810px;
      height:563px;
      position: absolute;
      top:50%;
      left:50%;
      margin-left: -405px;
      margin-top: -291px;
      -webkit-border-radius: 6px;
      -moz-border-radius: 6px;
      -webkit-border-radius: 6px;
      -moz-border-radius: 6px;
      border-radius: 6px;
      -webkit-box-shadow:  0px 0px 20px rgba(0,0,0,0.2);
      -moz-box-shadow:  0px 0px 20px rgba(0,0,0,0.2);
      box-shadow:  0px 0px 20px rgba(0,0,0,0.2);
      .title{
        color: #3C4858;
        font-weight: 700;
        font-size: 1.825em;
        line-height: 1.4em;
        text-align: center;
        padding: 20px 0px 30px 0px;
      }
      .main{
        padding:20px 15px;
        height: 405px;
        width:780px;
        overflow: auto;
      }
    }
  }
</style>
