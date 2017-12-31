<template>
  <div class="main1 text_note" style="display: none;">
    <section class="left">
      <div class="word">
        <label for="">Abbr to be confirmed</label>
        <p>JLAKFDAKLF</p>
      </div>
      <div class="textBox">
        <label for="">备注</label>
        <textarea></textarea>
        <div class="line"></div>
        <div class="strongLine"></div>
      </div>
    </section>
    <section class="right">
      <div class="labelBox">
        <label>context1</label>
        <p>"The room really nice name is recognized as being a really awesome room. We use it every sunday when we go fishing and we catch a lot. It has some kind of magic shield around it."</p>
      </div>
      <div class="labelBox">
        <label>context2</label>
        <p>"The room really nice name is recognized as being a really awesome room. We use it every sunday when we go fishing and we catch a lot. It has some kind of magic shield around it."</p>
      </div>
      <div class="labelBox">
        <label>context3</label>
        <p>"The room really nice name is recognized as being a really awesome room. We use it every sunday when we go fishing and we catch a lot. It has some kind of magic shield around it."</p>
      </div>
    </section>
    <div class="buttons">
      <div class="buttonsBox">
        <button>上一个</button>
        <button>修改</button>
        <button>提交</button>
        <button>下一个</button>
      </div>
    </div>
  </div>
</template>

<script>
  
</script>

<style scoped lang="less">
  @color3:#3399CC;
  .myLine(@top:-8px,@height:1px,@width:225px,@backgroudColor:#D2D2D2){
    position: relative;
    width: @width;
    height: @height;
    background-color: @backgroudColor;
    top: @top;
    margin:auto;
  }
  .main1{
    .right{
      float: left;
      height: 340px;
      width:530px;
      .labelBox{
        margin-bottom: 10px;
        label{
          color:#999999;
          font-size:13px;
        }
        p{
          width: 530px;
          height:70px;
          font-size:14px;
          overflow:auto;
        }
        p::-webkit-scrollbar{
          width:8px;
        }
        p::-webkit-scrollbar-thumb{
          width: 8px;
          -webkit-border-radius:;
          -moz-border-radius:;
          border-radius:10px;
          background-color: @color3;
        }
      }
    }
    .left{
      float: left;
      height:340px;
      width: 230px;
      padding-right: 20px;
      position: relative;
    }
    .buttons{
      clear: both;
      width:780px;
      height: 60px;
      .buttonsBox{
        margin:auto;
        position: relative;
        top:10px;
        button{
          width:140px;
          height:40px;
          background-color: @color3;
          text-align: center;
          line-height:40px;
          font-size: 17px;
          color: #FFFFFF;
          border:none;
          -webkit-border-radius: 3px;
          -moz-border-radius: 3px;
          border-radius: 3px;
          transition: box-shadow 0.2s cubic-bezier(0.4, 0, 1, 1), background-color 0.2s cubic-bezier(0.4, 0, 0.2, 1);
        }
        button:hover{
          box-shadow: 0 4px 4px 0 rgba(51, 153, 204, 0.14), 0 3px 1px -2px rgba(244, 67, 54, 0.2), 0 1px 5px 0 rgba(244, 67, 54, 0.12);
        }
      }
    }
  }
  .text_note{
    .left{
      .word{
        label{
          display: inline-block;
          color: #999999;
          font-size:12px;
        }
        p{
          height: 30px;
          line-height: 30px;
          font-weight: 700;
          font-size: 20px;
        }
      }
      .textBox{
        margin-top: 30px;
        label{
          display: inline-block;
          color: #999999;
          font-size:12px;
        }
        textarea{
          width:230px;
          height:175px;
          border:none;
          outline:none;
        }
        .line{
          .myLine(-5px);
        }
        .strongLine{
          .myLine(-6px,2px,0px,@color3)
        }
        textarea::-webkit-scrollbar{
          width:8px;
        }
        textarea::-webkit-scrollbar-thumb{
          width: 8px;
          background-color: @color3;
          -webkit-border-radius:10px;
          -moz-border-radius:10px;
          border-radius:10px;
          border-bottom: 1px solid #D2D2D2;
        }
      }
    }
    .buttons{
      .buttonsBox{
        width:580px;
      }
    }
  }
</style>