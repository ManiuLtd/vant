<style lang="less" scoped>
.chat {
	height: 100vh;
	width: 100%;
	overflow: hidden;
	display: flex;
	flex-direction: column;
	.top {
		flex: 1;
		overflow-y: auto;
		overflow-x: hidden;
		.item_right {
		   display: flex;
		   flex-direction: row;
		   align-items: center;
		   justify-content: flex-end;
		   padding: 15px;
		   .head {
		   	 width:35px;
		   	 height:35px;
		   	 margin-left: 20px;
		   }
		}
		.item_left {
		   display: flex;
		   flex-direction: row;
		   align-items: center;
		   justify-content: flex-start;
		   padding: 15px;
		   .head {
		   	 width:35px;
		   	 height:35px;
		   	 margin-right: 20px;
		   }
		}
		.send {
		   position:relative;
		   background:#F8C301;
		   border-radius:5px;
		   max-width: 300px;
		   padding:5px 10px;
		 }
		
		 .send .arrow {
		   position:absolute;
		   top:5px;
		   /*right:-16px;*/
		   width:0;
		   height:0;
		   font-size:0;
		   border:solid 8px;
		   /*border-color:transparent transparent transparent #F8C301;*/
		 }
		 .send .arrow_right {
		   right:-16px;
		   border-color:transparent transparent transparent #F8C301;
		 }
		 .send .arrow_left {
		   left:-16px;
		   border-color:transparent #F8C301 transparent transparent;
		 }
	}
	.footer {
		height: 50px;
		border-top: 1px solid rgba(204,204,204, .6);
		box-shadow: 0 0 4px rgba(128,128,128, .4);
		width: 100%;
		display: flex;
		flex-direction: row;
		align-items: center;
		padding: 0 10px;
		box-sizing: border-box;
		.image {
		   width: 30px;
		   height: 30px;
		}
		.button {
		   height: 25px;
		   width: 48px;
		   display: flex;
		   flex-direction: row;
		   align-items: center;
		   justify-content: center; 
		   background-color: green;
		   color:#fff;
		   font-size: 14px;
		   border-radius: 3px;
		}
		@keyframes changeBig {
          0% {
          	height: 0px;
          	width:0px;
          }
          100% {
          	height: 25px;
          	width:48px;
          }
        }
        .button_active {
        	  animation: changeBig .6s forwards;;
        }
		.inp {
		  border: none;
		  height: 30px;
		  width: 100%;
		  padding: 0 10px;
		  box-sizing: border-box;
		}
		.inp_box {
			border-bottom: 1px solid rgba(204,204,204, .6);
			flex: 1;
		}
	}
}
</style>
 
<template>
  <div class="chat">
  	<div class="top">
  	  <div :class="[id === todo.id ? 'item_right' : 'item_left']" v-for="(todo,index) in list" :key="index">
  	     <img :src="todo.headImg" v-show="id !== todo.id" class="head"/>
  	     <div class="send">
  	     	<span class="send_word">{{ todo.title }}</span>
  	     	<div :class="[id === todo.id ? 'arrow_right' : 'arrow_left', 'arrow']"></div>
  	     </div> 
  	     <img :src="todo.headImg" v-show="id === todo.id" class="head"/>
  	  </div>
  	</div>
  	<div class="footer">
  	  <img src="../../../static/images/yuyin.svg" class="image" />
  	  <div class="inp_box">
  	  	<input type="text" class="inp" v-model="word" />
  	  </div>
  	  <img src="../../../static/images/face.svg" class="image" style="margin-left: 5px;" />
  	  <img src="../../../static/images/add.svg" class="image" style="margin-left: 5px;" v-show="!word"/>
  	  <div v-show="word" :class="[word ? 'button_active' : '', 'button']" style="margin-left: 5px;" @click="send">
  	  	<span>发送</span>
  	  </div>
  	</div>
  </div>
</template>

<script>
   export default {
   	  data () {
   	  	return {
   	  	  word: '',
   	  	  id: 'boy',
   	  	  list: []
   	  	}
   	  },
   	  methods: {
   	  	async setData () {
   	  	  let obj = {};
   	  	  obj.title = this.word;
   	  	  obj.id = 'boy';
   	  	  obj.headImg = '../../../static/images/head-boy.png';
   	  	  if (!this.list) {
   	  	  	this.list = [];
   	  	  }
   	  	  this.list.push(obj);
   	  	  localStorage.setItem('chat', JSON.stringify(this.list));
   	  	},
   	  	async getData () {
   	  	  try {
   	  	  	let res = localStorage.getItem('chat'); 
   	  	  	this.list = JSON.parse(res);
   	  	  } catch(e) {
   	  	  	console.log(e)
   	  	  }
   	  	},
   	  	async send () {
   	  	  await this.setData();
   	  	  await this.getData();
   	  	  this.word = '';
   	  	}
   	  },
   	  async created () {
   	  	// 全局混入
   	  	this.setRouter();
   	  	await this.getData();
   	  	// js实现继承的方法
   	  	// 先有一个父类构造函数 
   	  	function father () {
   	  	   this.name = '父亲';
   	  	   this.sex = '男';
   	  	}
   	  	// 原型方法 
   	  	father.prototype.study = function () {
   	  	  console.log('父类的方法')
   	  	}
   	  	// 1. 原型链继承
   	  	function Son1() {
   	     
   	  	}
   	  	// 子类的原型等于父类的实例
   	  	Son1.prototype = new father()
   	  	let mySon = new Son1();
   	  	// 2. 构造继承
   	  	function Son2() {
   	  	  // 把子类的this通过传参传给父类的构造函数	
   	  	  father.call(this)
   	  	}
   	  	  	
   	  	// 3. 实例继承
   	  	// 在子类的构函数中return 父类的实例
   	  	function Son3() {
   	  	  var instant = new father()
   	  	  return instant
   	  	} 
   	  	
   	  	// sass 和 less   https://www.w3cplus.com/css/an-introduction-to-less-and-comparison-to-sass.html
   	  	// 混合， 参数混合，嵌套规则， 运算， 颜色功能，命名空间， 作用域， js表达式
   	 }
   }
</script>

