<!-- 以后项目的根组件 -->
<template>
<div>
<!--   <p class="jj" v-on:click="text()">hhhha</p>-->
 
  <!-- 2.0 利用vue-router的 <router-view>进行占位 -->
  	<div class="main">
  		<div class="m_img">
  			<div class="m_box">
  				<div class="logo">
  			
  					<img src="../statics/imgs/logo.png" alt="" />
  				</div>
  				<div class="say">
  					<span>为爱保鲜，恋人必备，全宇宙的CP都在玩！</span>
  					<img src="../statics/imgs/say.png" alt="" />
  				</div>
  			</div>
  			<div class="content">
  				<div class="track">
  					<img src="../statics/imgs/track.png" alt="" />
  				</div>
  				<div class="raido">
  					<img src="../statics/imgs/raido.png" alt="" />
  				</div>
  				<div id="test_a"class="test">
  					<img src="../statics/imgs/test.png" alt="" />
  				</div>
  				<div id="maol_a" class="maol">
  					<img src="../statics/imgs/maol.png" alt="" />
  				</div>
  				<div id="leaf_a" class="leaf">
  					<img src="../statics/imgs/leaf.png" alt="" />
  				</div>
  				<div class="coin_top">
  					<img src="../statics/imgs/coin_top.png" alt="" />
  				</div>
  				<div id="coin_a" class="coin_bottom">
  					<img src="../statics/imgs/coin_bottom.png" alt="" />
  				</div>
  			</div>
  			<div id="detial" style="">
  				<img src="../statics/imgs/detil.png" alt="" />
  				<div class="d_tip">
  				   <div class="d_left">
  						<img v-bind:src="banimg" alt="" />
  				   </div>
  				   <div class="d_right">
  					<p class="d_wenzi">{{creator_name}}和TA的伴侣</p>
  					<p>准备用{{needs_days}}个月去实现共同愿望</p>
  				   </div>
  				</div>
  				   <div class="d_cont">
  				   	<p>{{wish_name}}</p><!--<p>去地球的尽头，</p>  <p>去看呐美丽的极光。</p>-->
  				   </div>
  					<!--<div>
  						
  					</div>-->
  					
  			</div>
  		</div>
  	</div>
  	<router-view></router-view>

  <!-- 3.0 利用mui中的tabbar组件实现系统的底部 -->

  <div class="bottom">
  	<div class="b_title">
  		<h4>活动细则</h4><div class="title_img"><img src="../statics/imgs/title.png" alt="" /></div>
  	</div>
  	<ul>
  		<li><p>	1、愿望每获得一次点赞，分享的情侣和助力的好友都能获得红包。</p></li>
  		<li><p>2、助力的用户每日获得的红包上限为5个。 </p></li>
  		<li style="background:darkgoldenrod;width:300px;height:20px;text-align:center;"><p>{{wuid}}</p></li>
  		<li><p>{{thirdparty_no}}</p></li>
  		<li><p>3、分享愿望的情侣每日获得的红包上限为20个。</p></li>
  		<li><p>4、新用户需要注册并绑定为情侣后，才可以领取红包。</p></li>
  		<li><p>5、红包在恋爱宝首页领取后，自动转换为恋爱基金。</p></li>
  		<li><p>6、使用红包的手机号须为注册时使用的手机号。</p></li>
  		<li><p> 7、本活动最终解释权归恋爱宝所有。</p></li>
  	</ul>
    
  </div>
</div>
</template>

<script>
import $ from '../statics/js/jquery-3.2.1.min.js';
import common from './kits/common.js';
export default { // es6的导出对象的写法
  data() { //等价于 es5的 data:function(){
    return {
    	wuid:'',
    	thirdparty_no:'',
    	banimg:'',
		 list:null,
		 needs_days:'',
		 creator_name:'',
		 wish_name:'',
    }
  },
   created() {
// 	先不启用
		      function UrlSearch() 
              {
                
              var name,value; 
              var str=location.href; //取得整个地址栏
              var num=str.indexOf("?") 
              str=str.substr(num+1); //取得所有参数   stringvar.substr(start [, length ]

              var arr=str.split("&"); //各个参数放到数组里
              for(var i=0;i < arr.length;i++){ 
               num=arr[i].indexOf("="); 
               if(num>0){ 
                name=arr[i].substring(0,num);
                value=arr[i].substr(num+1);
                this[name]=value;
              } 
            } 
        } 
        var Request=new UrlSearch(); //实例化
        var slicwuid = Request.wuid;
        //截取问好后面参数#号前面的参数
          this.wuid=slicwuid.substr(0, slicwuid.indexOf('#'));
          this.thirdparty_no=Request.thirdparty_no;
          this.getBanner();
  },
  methods: {
    text: function() {
      //					'color':'red','font-size':'12px'
      $(".jj").css('transform', "rotate(90deg)");

    },
    getBanner:function(){
        var that=this;
    $.ajax({
        url:common.apidomain+"/activity/share_wish/get",
        dataType: 'json',
        data:{
           		wuid:that.wuid,
	 			thirdparty_no:that.thirdparty_no,
	 			thirdparty_type:2
            },
        success:function(data){
            if(data.code==10000){
//         	console.log(data);
                that.list=data.data.wish_user_generate_d_t_o;
             	console.log(that.list);
                that.needs_days=that.list.needs_days/30;
                that.creator_name=that.list.creator_name;
                that.wish_name=that.list.wish_name;
                that.banimg=that.list.img_url;
                
//               var id=data.data.user.mate_grade_id;
//               var f=Math.round(data.data.user.coin_percent*100); 
                }
               }
             })
     },
    //过渡动画的效果maoltop: 0px;，test top: -40px;，coin_bottomtop: 2.5rem;  
  }
 
	
}
</script>

<style scoped>
/*当前页面的css样式写到这里，其中scoped表示这个里面写的css代码只是在当前组件页面上有效，不会去影响到其他组件页面*/
body{
	font-size: 12px;
}
.main {
  width: 100%;
  height: 32.69rem;
 
  overflow: hidden;
/*  background: rgb(101, 207, 148);*/
  /*	line-height: 200px;*/
  /*padding-top: 50px;
  text-align: center;*/

}
.m_box{
	width:100%;

	overflow: hidden;
}
.main .m_img{
	width:100%;
	height:100%;
	overflow: hidden;
	/*background-position: center center;*/
	background-image: url(../statics/imgs/main.png);
  	background-repeat:no-repeat;
  	background-size: cover;
  	position: relative;
}
.m_img .logo{
	
	float: left;
	width:20%;
	height: 1.68rem;
	margin:0.75rem;

	background-position: 50% 50%;
/*	background-image: url(../statics/imgs/logo.png);*/
	background-size: cover;
}
.logo>img{
	background-position: center center;
	background-size: cover;
	width:100%;
	height:100%;
}
.m_img .say{
	margin:0.75rem 0;
	float: left;
	width: 69.6%;
	height: 1.58rem;

	position: relative;
	z-index: 2;
}
.say>span{
	position: absolute;
	top:0;
	left:0;
	right: 0;
	bottom: 0;
	margin: 0;
	text-align: center;
	line-height:1.58rem;
	font-size: 0.65rem;
	color:#A4AFFF;
	font-weight: 700;	
}
.say>img{
	background-position: center center;
	background-size: cover;
	width:100%;
	height:100%;
	
}

.main>p {
/*  display: block;*/
  /*	line-height: 0px;*/
}
.content{
	width:100%;
/*	height: 50rem;*/
	position: relative;

}
.raido,
.track,
.test,
.maol,
.leaf,
.coin_top,
.coin_bottom{
	position: absolute;
	
}
.raido,
.track,
.test,
.maol,
.leaf,
.coin_top,
.coin_bottom>img{
	background-position: center center;
	background-size: cover;
	width:100%;
	height:100%;
}

.track{
/*	border: 1px solid firebrick;*/
	width:16.63rem;
	height: 7.5rem;

	z-index: 2;
}
.raido{
	/*border: 1px solid red;*/
	width: 19.72rem;
	height: 7.32rem;
	right: 0;
	top: 0.31rem;
}
.test{
	/*border: 1px solid darkgoldenrod;*/
	width: 5.28rem;
	height: 4.05rem;
	top: -40px;
    right:0.75rem;
   /* transition: all 3s;*/
     animation: run 5s linear infinite;
}
@keyframes run {
			/*百分比是相对于动画的执行时间*/
			0% {
				transform: translate(0, 0px) ;
				
			}
			15% {
		/*		width: 400px;
				height: 200px;*/
				transform: translate(0, 4px);
			
			}
			30% {
		/*		width: 400px;
				height: 200px;*/
				transform: translate(0, 7px);
	
			}

			45% {
/*				width: 400px;
				height: 400px;*/
			
					 transform: translate(0, 10px);
			}
			60% {
/*				width: 400px;
				height: 400px;*/
				 transform: translate(0, 7px);
			
			}

			85% {
/*				width: 400px;
				height: 400px;
				background-color: yellow;*/
					transform: translate(0, 4px);
			
			}

			100% {
			/*	width: 200px;
				height: 200px;*/
		/*		background-color: green;*/
					transform: translate(0, 0px) ;
			}
		}

.maol{
/*	border: 1px solid blue;*/
	width: 9.05rem;
	height: 7.9rem;
	top: 0px;
    left: 3.13rem;
    right: 0;
 
    margin: 0 auto;
    animation: maol 4s linear infinite;

}
@keyframes maol {
			/*百分比是相对于动画的执行时间*/
			0% {
				transform: translate(0, 0px) ;
				
			}
			15% {
		/*		width: 400px;
				height: 200px;*/
				transform: translate(0, 4px);
			
			}
			30% {
		/*		width: 400px;
				height: 200px;*/
				transform: translate(0, 6px);
	
			}

			45% {
/*				width: 400px;
				height: 400px;*/
			
					 transform: translate(0, 8px);
			}
			60% {
/*				width: 400px;
				height: 400px;*/
				 transform: translate(0, 6px);
			
			}

			85% {
/*				width: 400px;
				height: 400px;
				background-color: yellow;*/
					transform: translate(0, 4px);
			
			}

			100% {
			/*	width: 200px;
				height: 200px;*/
		/*		background-color: green;*/
					transform: translate(0, 0px) ;
			}
		}

.leaf{

/*	border: 1px solid palevioletred;*/
	width: 7.15rem;
	height: 13rem;
	 right: -1rem;
    top:3.5rem;
    z-index: 3;
    animation: change 9s linear infinite;
}
@keyframes change {
			/*百分比是相对于动画的执行时间*/
			0% {
						transform: rotateZ(-0deg);
				
			}
			15% {
		/*		width: 400px;
				height: 200px;*/
				transform: rotateZ(-5deg);
			}
			30% {
		/*		width: 400px;
				height: 200px;*/
				transform: rotateZ(-10deg);
			}

			45% {
/*				width: 400px;
				height: 400px;*/
					transform: rotateZ(-15deg);
			}
			60% {
/*				width: 400px;
				height: 400px;*/
					transform: rotateZ(-10deg);
			}

			85% {
/*				width: 400px;
				height: 400px;
				background-color: yellow;*/
					transform: rotateZ(-5deg);
			}

			100% {
			/*	width: 200px;
				height: 200px;*/
		/*		background-color: green;*/
			transform: rotateZ(0deg);
			}
		}
.coin_top{
/*	border: 1px solid black;*/
	width:3.7rem;
	height:3.5rem;
	top: 0.75rem;
    left: 5.25rem;
}
.coin_bottom{
/*	border: 1px solid mediumturquoise;*/
	width: 8.31rem;
	height: 8.9rem;
	top: 4.5rem;
	z-index: 2;
	 animation: coin 6s linear infinite;
}
@keyframes coin {
			/*百分比是相对于动画的执行时间*/
			0% {
				transform: translate(0, 0px) ;
				
			}
			15% {
		/*		width: 400px;
				height: 200px;*/
				transform: translate(0, 5px);
			
			}
			30% {
		/*		width: 400px;
				height: 200px;*/
				transform: translate(0, 10px);
	
			}

			45% {
/*				width: 400px;
				height: 400px;*/
			
					 transform: translate(0, 14px);
			}
			60% {
/*				width: 400px;
				height: 400px;*/
				 transform: translate(0, 10px);
			
			}

			85% {
/*				width: 400px;
				height: 400px;
				background-color: yellow;*/
					transform: translate(0, 5px);
			
			}

			100% {
			/*	width: 200px;
				height: 200px;*/
		/*		background-color: green;*/
					transform: translate(0, 0px) ;
			}
		}
#detial{
	padding: 1.25rem;
	position: absolute;
	height:19.2rem;
	/*    width: 20.94rem;*/
	bottom:0.4rem;
	left: 0;
	right: 0;
	/*border: 1px solid #394740;*/
	background-position: center center;
	/*background-image: url(../statics/imgs/detil.png);*/
  	background-repeat:no-repeat;
  	background-size: cover;
  	margin: auto;

}
#detial>img{
	width:100%;
	height: 100%;
}
#detial .d_tip{
	/*border: 1px solid #394740;*/
	overflow: hidden;
	width:100%;
	height:6.87rem;
	padding: 1.4rem;
	position: absolute;
    top: 1.5rem;
    left: 1.4rem;
	
}
#detial .d_tip .d_left{
	width: 4.06rem;
    height: 4.06rem;
    background: #fff;
    float: left;
    border-radius: 50%;
    overflow: hidden;
	
	
}
#detial .d_tip .d_left>img{
	width:100px;
}
#detial .d_tip .d_right{
	float: left;
   /* margin-top: 0.75rem;*/
    margin-left: 0.8rem;
    color:
	
}
#detial .d_tip .d_right>p{
	color: #fff;
	font-size: 0.75rem;
	margin-top: 0.6rem;
}
#detial .d_tip .d_right .d_wenzi{
	font-size: 1rem;
	margin-top: 0.6rem;
}
.d_cont{
	width: 11.25rem;
    height: 6.75rem;
    margin-left: 3.25rem;
    position: absolute;
    bottom:3rem;
	transition: all 3s ease-out 3s;
  	animation: d_cont 3s linear forwards;
}
  @keyframes d_cont {
      /*百分比是相对于动画的执行时间*/
      0% {
        opacity:0;
        
      }

       100% {
   
       	transition: all 3s ease-out 3s;
      	opacity:1;
      }
    }
.d_cont>p{
	font-size: 1rem;
    color: #fff;
    font-weight: 700;
    font-family: 'PingFangSC';
    line-height: 2.25rem;
}

.bottom {
  height: 24.5rem;

    background: rgba(26,9,83,1);
}
.bottom .b_title{
	height: 1.25rem;
	text-align: center;
	
}
.b_title>h4{
	color:#fff;
	font-size:0.875rem;
	width: 20%;
    float: left;
	line-height: 1.25rem;
	margin-left: 2.25rem;
}
.b_title>.title_img{
	height: 1.25rem;
    width: 68%;
	 float: left;
}
.title_img>img{
	background-position: center center;

  	background-repeat:no-repeat;
  	background-size: cover;
  	margin: auto;
  	width:100%;
}
	
.bottom>ul{
	text-align:center;
	width: 76%;
  /*  border: 1px solid #ccc;*/
    margin: 0 auto;
    margin-top: 0.6rem;
}
.bottom>ul>li>p{
	color:#fff;
	font-size: 0.75rem;
	line-height: 1.5rem;
	text-align:left;
}

</style>
