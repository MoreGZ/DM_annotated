<template>
	<div class="main1 word_note">
        <section class="left">
            <p>
                {{getData[currentPage-1]['word']}}
                <span v-for="(word,index) in getData2[currentPage-1]">
                    {{word}} 
                    <img src="../assets/f00d.png" height="512" width="512"  v-on:click="deleteData2(index,word)"/>
                </span>
            </p>
            <div class="selectBox">
                <label for="" id="type_label" v-bind:style="{transform:typePosition}">type</label>
                <select name="" id="" value="" v-on:change="addNote1();clearTitle()" v-model="editData[currentPage-1]['type']" v-on:focus="focusAnimate2" v-on:blur="blurAnimate2">
                    <option value=""></option>
                    <option value="name">人名</option>
                    <option value="organization">组织、机构名</option>
                    <option value="local">地方名</option>
                    <option value="entity">其他实体</option>
                    <option value="abbr">缩略语</option>
                    <option value="error">文本错误</option>
                    <option value="foreign">外来语</option>
                    <option value="plural">复数名词</option>
                    <option value="phrase">固定词组</option>
                    <option value="emphasis">表强调</option>
                    <option value="other">其他</option>
                </select>
                <div class="line"></div>
                <div class="strongLine" v-bind:style="{width:width2+'px',transition: 'width 0.2s linear'}"></div>
            </div>
            <div class="textBox">
                <label for="">备注</label>
                <textarea 
                	v-on:change="addNote2" 
                	v-model="editData[currentPage-1]['note']" 
                	v-bind:readonly="ifDisable" 
                	v-on:focus="focusAnimate1" 
                	v-on:blur="blurAnimate1">
                </textarea>
                <div class="line"></div>
                <div class="strongLine" v-bind:style="{width:width1+'px',transition: 'width 0.2s linear'}"></div>
            </div>
            <div class="addText">
                <label for="">{{nowTitle}}</label>
                <input type="text" 
                	v-bind:readonly="ifDisable2" 
                	v-on:focus="focusAnimate4" 
                	v-on:blur="blurAnimate4"
                	v-model="editData[currentPage-1]['addition']"
                	v-on:change="addNote1">
                </input>
                <div class="line"></div>
                <div class="strongLine" v-bind:style="{width:width4+'px',transition: 'width 0.2s linear'}"></div>
            </div>
            <div class="addText">
                <label for="">顺便标注</label>
                <input type="text" v-model="inputData" v-on:keyup.enter="addData2" v-on:keyup.esc="cansolAddData2" v-on:focus="focusAnimate3" v-on:blur="blurAnimate3"></input>
                <div class="line"></div>
                <div class="strongLine" v-bind:style="{width:width3+'px',transition: 'width 0.2s linear'}"></div>
            </div>
        </section>
        <section class="right">
            <div class="labelBox">
                <label>context1</label>
                <p id="p1">{{getData[currentPage-1]['context'][0]}}</p>
            </div>
            <div class="labelBox">
                <label>context2</label>
                <p id="p2">{{getData[currentPage-1]['context'][1]}}</p>
            </div>
            <div class="labelBox">
                <label>context3</label>
                <p id="p3">{{getData[currentPage-1]['context'][2]}}</p>
            </div>
        </section>
        <div class="buttons" style="position: relative;">
            <span style="position: absolute;left: 0px;top:20px;font-size: 14px">你总共已标注<span style="color:#39c;font-size:16px;font-weight:800;">{{count}}</span>个词</span>
            <div class="buttonsBox">
                <button v-on:click="lastPageHandle">上一个</button>
                <button v-on:click="sendDataHandle">提交</button>
                <button v-on:click="nextPageHandle">下一个</button>
            </div>
            <span style="position: absolute;right: 0px;top:20px;font-size: 20px">{{currentPage}}/{{getData.length}}</span>
        </div>
    </div>
</template>

<script>
	import $ from 'jquery'
	export default {
		props:["username"],
	    data:function(){
	        return {
	        	// 获取的数据组的数据源
	            getData:[],
	            //储存每一页的顺便标注
	            getData2:[],

	            //要发送的已经标注好的数据集（已有数据集和顺便标注）
	            sendData:[],
	            sendData2:[],

	            //记录存在于sendData中的数据的索引
	            sendDataA:[],

	            // 当前数据组的编辑记录
	            editData:[],

	            //当前页面
	            currentPage:1,

	            // 动画横线的长度
	            width1:0,
	            width2:0,
	            width3:0,
	            width4:0,

	            //统计当前用户的总标注数量
	            count:"",

	            // 顺便标注的输入框的内容
	            inputData:"",

	            //补齐标题
	            titleObj:{
	            	"name":"补齐完整",
	            	"organization":"补齐完整",
	            	"local":"补齐完整",
	            	"abbr":"请给出完整词组",
	            	"error":"请给出拼写正确的词",
	            	"entity":"null",
	            	"foreign":"null",
	            	"plural":"null",
	            	"phrase":"null",
	            	"emphasis":"null",
	            	"other":"null",
	            	"":"null"
	            }
	        }
	    },
	    computed:{
	        ifDisable:function () {
	            if(this.editData[this.currentPage-1]['type']=="other"){
	                return false;
	            }else{
	                return true;
	            }
	        },
	        nowType:function(){
	        	return this.editData[this.currentPage-1].type;
	        },
	        nowTitle:function(){
	        	return this.titleObj[this.nowType];
	        },
	        ifDisable2:function(){
	        	return this.nowTitle=="null" ? true : false ;
	        },
	        ifInSendData:function () {
	            var a = false;
	            for(var x=0;x<this.sendDataA.length;x++){
	                if(this.sendDataA[x]==this.currentPage) return true;
	            }
	            return a;
	        },
	        typePosition:function () {
	            if(this.editData[this.currentPage-1]['type']==""){
	                return 'translate(0,25px)';
	            }else {
	                return 'translate(0,1px)';
	            }
	        }
	    },
	    methods:{
	        sendDataHandle:function(){
	            var vm = this;
	            console.log(this.sendData.length);
	            console.log(this.sendData2);
	            $.post("/label/home/index/save",{
	                data:this.sendData,
	                data2:this.sendData2,
	            },function(data,status){
	                var a = JSON.parse(data);
	                if(a.status==1){
	                    //alert("提交成功");
	                    vm.sendData = [];
	                    vm.sendData2 = [];
	                    vm.sendDataA = [];
	                }else{
	                    alert("提交失败");
	                }

	                vm.count = a.count;
	                console.log(vm.count);
	            })
	        },
	        getDataHandle:function(){
	            var vm = this;
	            $.post("/api/data",{
	            },function(data,status){
	                vm.getData = [];
	                vm.editData = [];
	                var a = JSON.parse(data);
	                for (var i=0;i<a.data.length;i++) {
	                    vm.getData.push(a.data[i]);
	                    var obj = {};
	                    obj.id = a.data[i]["id"];
	                    obj.type = "",
	                    obj.note = "",
	                    obj.addition = "",
	                    obj.annotator=vm.username;
	                    vm.editData.push(obj);
	                }
	                //数据组接收，编辑数据初始化完成
	                console.log("接收成功")
	                vm.createGetData2();

	                vm.count = a.count;
	            })
	        },
	        lastPageHandle:function(){
	            if(this.currentPage==1){
	                alert("已经是第一页了")
	            }else {
	                this.currentPage--;
	            }
	        },
	        nextPageHandle:function(){
	            if(this.currentPage==this.getData.length){
	                var vm = this;
	                var c = confirm("已经是最后一页了，请问是否获取下一个数据集")
	                if(c){
	                    if(this.sendData.length>0){
	                        var c2 = confirm("有数据没有提交，是否提交数据");
	                        if(c2){
	                            $.post("/label/home/index/save",{
	                                data2:vm.sendData2,
	                                data:vm.sendData
	                            },function(data,status){
	                                var a=JSON.parse(data);
	                                if(a.status==1){
	                                    //alert("提交成功");
	                                    vm.sendData = [];
	                                    vm.sendData2 = [];
	                                    vm.sendDataA = [];
	                                    vm.getDataHandle();
	                                }else{
	                                    alert("提交失败");
	                                }
	                            })
	                        }else {
	                            vm.getDataHandle();
	                        }
	                    }else{
	                        vm.getDataHandle();
	                    }
	                    this.currentPage = 1;
	                }
	            }else {
	                this.currentPage++;
	            }
	        },
	        addNote1:function(){
	            if(!this.ifInSendData){
	                this.sendData.push(this.editData[this.currentPage-1])
	                this.sendDataA.push(this.currentPage);
	            }else{
	                for(var x=0;x<this.sendDataA.length;x++){
	                    if(this.sendDataA[x]==this.currentPage){
	                       this.sendData[x]=this.editData[this.currentPage-1]
	                    }
	                }
	            }
	            if(!(this.editData[this.currentPage-1]['type']=="other")){
	                this.editData[this.currentPage-1]['note']=""
	            }
	            this.addNote4();
	            console.log(this.sendData);
	            console.log(this.sendData2);
	            this.checkWord2();
	        }, 
	        addNote2:function(){
	            if(!this.ifInSendData){
	                this.sendData.push(this.editData[this.currentPage-1])
	                this.sendDataA.push(this.currentPage);
	            }else {
	                for(var x=0;x<this.sendDataA.length;x++){
	                    if(this.sendDataA[x]==this.currentPage){
	                        this.sendData[x]=this.editData[this.currentPage-1]
	                    }
	                }
	            }
	            this.addNote4();
	            console.log(this.sendData);
	            console.log(this.sendData2);
	            this.checkWord2();
	        },
	        addNote3:function(word){
	            var k = {};
	            var vm = this;
	            k.word = word;
	            k.type = this.editData[this.currentPage-1]["type"],
	            k.note = this.editData[this.currentPage-1]["note"],
	            k.addition = this.editData[this.currentPage-1]["addition"];
	            k.annotator=vm.username;
	            this.sendData2.push(k);
	        },
	        addNote4:function(){
	            for(var i=0;i<this.getData2[this.currentPage-1].length;i++){
	                var k = {};
	                var vm = this;
	                k.word = this.getData2[this.currentPage-1][i];
	                k.type = this.editData[this.currentPage-1]["type"],
	                k.note = this.editData[this.currentPage-1]["note"],
	            	k.addition = this.editData[this.currentPage-1]["addition"];
	                k.annotator=vm.username;
	                this.sendData2.push(k);
	            }
	        },
	        clearTitle:function(){
				this.editData[this.currentPage-1].addition = "";
	        },
	        deleteNote:function(word){
	            for(var i=0;i<this.sendData2.length;i++){
	                if(this.sendData2[i]['word']==word){
	                    this.sendData2.splice(i, 1);
	                }
	            }
	        },
	        blurAnimate1:function () {
	            this.width1 = 0;
	        },
	        focusAnimate1:function () {
	            if(!this.ifDisable){
	                this.width1 = 230
	            }
	        },
	        blurAnimate2:function () {
	            this.width2 = 0;
	        },
	        focusAnimate2:function () {
	            this.width2 = 230
	        },
	        blurAnimate3:function () {
	            this.width3 = 0;
	        },
	        focusAnimate3:function () {
	            this.width3 = 230
	        },
	        blurAnimate4:function () {
	        	this.width4 = 0;
	        },
	        focusAnimate4:function () {
	            if(!this.ifDisable2){
	        		this.width4 = 230;
	        	}
	        },
	        findKeyWord:function () {
	            var a = this.getData[this.currentPage-1]['word'];
	            var p1 = this.getData[this.currentPage-1]['context'][0];
	            var p2 = this.getData[this.currentPage-1]['context'][1];
	            var p3 = this.getData[this.currentPage-1]['context'][2];
	            document.getElementById("p1").innerHTML = p1.replace(a, "<span style='color: red;font-weight: 800;font-size: 15px;'>"+" "+a+""+"</span>");
	            document.getElementById("p2").innerHTML = p2.replace(a, "<span style='color: red;font-weight: 800;font-size: 15px;'>"+" "+a+""+"</span>");
	            document.getElementById("p3").innerHTML = p3.replace(a, "<span style='color: red;font-weight: 800;font-size: 15px;'>"+" "+a+""+"</span>");
	            // 将书便标注的词标红
	            for(var i=0;i<this.getData2[this.currentPage-1].length;i++){
	                var b = this.getData2[this.currentPage-1][i];
	                var p1 = document.getElementById("p1").innerHTML;
	                var p2 = document.getElementById("p2").innerHTML;
	                var p3 = document.getElementById("p3").innerHTML;
	                document.getElementById("p1").innerHTML = p1.replace(b, "<span style='color: red;font-weight: 800;font-size: 15px;'>"+" "+b+""+"</span>");
	                document.getElementById("p2").innerHTML = p2.replace(b, "<span style='color: red;font-weight: 800;font-size: 15px;'>"+" "+b+""+"</span>");
	                document.getElementById("p3").innerHTML = p3.replace(b, "<span style='color: red;font-weight: 800;font-size: 15px;'>"+" "+b+""+"</span>");
	            }
	        },
	        createGetData2:function(){
	            for(var i=0;i<this.getData.length;i++){
	                this.getData2[i] = [];
	            }
	            // console.log(this.getData2);
	        },
	        deleteData2:function(index,item){  //删除顺便标注
	            this.deleteNote(item);
	            this.getData2[this.currentPage-1].splice(index,1);
	            console.log(this.sendData);
	            console.log(this.sendData2);
	            this.focusAnimate1();
	            this.focusAnimate2();
	            this.blurAnimate1();
	            this.blurAnimate2();
	        },
	        addData2:function(e){
	            if(this.inputData!=""){
	                this.getData2[this.currentPage-1].push(this.inputData);
	                this.checkWord(this.inputData); // 过滤一下顺便标注的词是不是现在就在目前的接收到的词中
	                this.addNote3(this.inputData);
	                this.inputData = "";
	                console.log(this.sendData);
	                console.log(this.sendData2);
	            }

	        },
	        cansolAddData2:function(e){
	            this.inputData = "";
	        },
	        checkWord:function(word){ // 过滤一下顺便标注的词是不是现在就在目前的接收到的词中
	            for(var i=0;i<this.getData.length;i++){
	                if(word==this.getData[i]['word']){
	                    this.editData[i]['type'] = this.editData[this.currentPage-1]['type'];
	                    this.editData[i]['remark'] = this.editData[this.currentPage-1]['remark'];
	                    return true
	                }
	            }
	            return false;
	        },
	        checkWord2:function(){ // 过滤一下顺便标注的词是不是现在就在目前的接收到的词中
	            for(var x=0;x<this.getData2[this.currentPage-1].length;x++){
	                this.checkWord(this.getData2[this.currentPage-1][x])
	            }  
	        }
	    },
	    created:function(){
	        // 获取数据
	        var vm = this;
	        vm.getDataHandle();
	    },
	    updated:function(){
	        this.findKeyWord();
	    }
	}
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
            height:84px;
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
    .word_note{
      .left{
        .selectBox{
          padding-bottom: 5px;
          margin: 5px 0 0 0;
          label{
            display: inline-block;
            color: #999999;
            font-size:14px;
            -webkit-transform: translate(0,25px);
            -moz-transform: translate(0,25px);
            -ms-transform: translate(0,25px);
            -o-transform: translate(0,25px);
            transform: translate(0,25px);
            transition: all 0.2s ease-out;
          }
          select{
            color: #555;
            display: block;
            height: 36px;
            padding: 7px 0;
            font-size: 14px;
            line-height: 1.42857;
            font-weight: 400;
            background-color: transparent;
            border: 0;
            margin-bottom: 7px;
            width:230px;
            outline:none;
          }
          .line{
            .myLine();
          }
          .strongLine{
            .myLine(-11px,2px,0px,@color3);
          }
        }
        p{
          font-size: 20px;
          font-weight: 700;
          img{
          	width: 15px;
          	height: 15px;
          	cursor:  pointer;
          	position: relative;
          	top:1px;
          }
        }
        .addText{
          label{
            display: inline-block;
            color: #999999;
            font-size:12px;
          }
          input{
            width:230px;
            height:40px;
            border:none;
            outline:none;
          }
          .line{
            .myLine(-5px);
          }
          .strongLine{
            -webkit-transition: width 0.2s linear;
            -moz-transition: width 0.2s linear;
            -ms-transition: width 0.2s linear;
            -o-transition: width 0.2s linear;
            transition: width 0.2s linear;
            .myLine(-6px,2px,0px,@color3)
          }
        }
        .textBox{
          margin-top: 0px;
          label{
            display: inline-block;
            color: #999999;
            font-size:12px;
          }
          textarea{
            width:230px;
            height:80px;
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
          width:430px;
        }
      }
    }
</style>