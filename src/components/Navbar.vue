<template>
	<div class="tabs">
		<ul>
			<li v-on:click="changeTabLeft"><a href="#">类型标注</a></li>
			<li v-on:click="changeTabRight"><a href="#">缩略语标注</a></li>
		</ul>
		<div class="slider">{{currentTypeText[currentType]}}</div>
	</div>
</template>

<script>
	import $ from 'jquery'
	export default {
		data:function(){
	        return{
	            currentType:0,
	            currentTypeText:["类型标注","缩略语标注"]
	        }  
	    },
	    methods:{
	        changeTabLeft:function(){
	            if(this.currentType==1){
	                $(".slider").animate({left:'-40px'},300);
	                $(".slider").animate({left:'-10px'},100);
	                this.currentType=0
	                this.$emit("turn_left")
	            }
	        },
	        changeTabRight:function(){
	            if(this.currentType==0){
	                $(".slider").animate({left:'440px'},300);
	                $(".slider").animate({left:'410px'},100);
	                this.currentType=1;
	                this.$emit("turn_right");
	            }
	        }
	    },
	}
	
</script>

<style scoped lang="less">
	@color3:#3399CC;
	.tabs{
		position: relative;
		ul{
			background-color: rgba(200, 200, 200, 0.2);
			overflow: auto;
			li{
				list-style: none;
				width: 50%;
				text-align: center;
				float: left;
			a{
				border: 0 !important;
				border-radius: 0;
				line-height: 18px;
				text-transform: uppercase;
				font-size: 16px;
				font-weight: 500;
				min-width: 100px;
				text-align: center;
				color: #555555 !important;
				text-decoration: none;
				position: relative;
				display: block;
				padding: 10px 15px;
			}
		}
		}
		.slider{
			position: absolute;
			text-align: center;
			padding: 12px;
			font-size: 17px;
			text-transform: uppercase;
			-webkit-font-smoothing: subpixel-antialiased;
			top: -4px;
			left: -10px;
			border-radius: 4px;
			color: #FFFFFF;
			cursor: pointer;
			font-weight: 500;
			background-color: @color3;
			box-shadow: 0 16px 26px -10px #3399CC, 0 4px 25px 0px rgba(0, 0, 0, 0.12), 0 8px 10px -5px rgba(244, 67, 54, 0.2);
			width: 381px;
			height: 21px;
		}
	}	
</style>