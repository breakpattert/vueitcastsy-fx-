<template>
	<div id="tmpl">
		<!--1.0 实现新闻资讯列表样式-->
		<div class="mui-content">
			<div class="c_bgc">
				<div class="tips_h">
					<img src="../../../statics/imgs/tip_h.png" alt="" />
				</div>
				<div class="c_input">
					<div class="tip_detil">
						<h3>恭喜获得助力红包</h3>
						<p>红包已放至恋爱宝，登录领取</p>
						
					</div>
					<div class="money">
							￥<span>{{amount}}</span>
					</div>
					
					<img src="../../../statics/imgs/hongbao.png" alt="" />
				</div>
    			<a class="btn-download" href="http://a.app.qq.com/o/simple.jsp?pkgname=me.fmfm.loverfund">
    				<h4>打开恋爱宝APP</h4>
    				<img src="../../../statics/imgs/lianai.png" alt="" />
    			</a>
    		</div>
    		<div class="buttom_bgc">
    			<div class="change_s">
    				<label>红包将放至账户</label>
    				<span>{{userphone}}</span>
    				<a v-on:click="changePhone()" style="padding-bottom: 3px;border-bottom: 1px solid #fecb85;">修改></a>
    			</div>
    			
    		</div>
			
		
    			
    			
    		
			<div class="footer">
      	
     			<div class="tip_title">
  					<h4>{{point}}位好友为他们的愿望助了一份力</h4><div class="tip_img"><img src="../../../statics/imgs/tip_q.png" alt="" /></div>
  				</div>
      	
      	
    			<p>已经有{{point}}位好友支持他们</p>
    			<ul>
      				<li v-for="item in userlist"><img v-bind:src="item.img_url" alt="" /></li>
    
      		
    			</ul>
    			<div class="ridio_d">
    				<div class="ridio_li ridio_left" ><a href=""></a></div>
      				<div  class="ridio_li"><a href=""></a></div>
      				<div  class="ridio_li"><a href=""></a></div>
    			</div>
    	
    	
  			</div>
		</div>

	</div>
</template>

<script>
import { Toast } from 'mint-ui';
import common from '../../kits/common.js';

import $ from '../../../statics/js/jquery-3.2.1.min.js';

	export default{
		data(){
			return {
//				list:[], //新闻列表功能
				userphone:'',
				lister:null,
				amount:'',
				point:'',
				userlist:null,
				wuid:'',
    			thirdparty_no:'',
			}
		},
		created(){
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
			this.getphone();
			this.getmoney();
			this.getuserlike();
		},
		methods:{
        	getmoney:function(){
        var that=this;
    	$.ajax({
        	url:common.apidomain+"/activity/share_wish/account/get",
        	dataType: 'json',
        	data:{
           		wuid:that.wuid,
				thirdparty_no:that.thirdparty_no,
	 			thirdparty_type:0
            },
        	success:function(data){
            if(data.code==10000){
            	
//       	console.log(data);
         	that.lister=data.data.activity_thirdparty_user;
         	console.log(2222);
         	console.log(that.lister);
         	that.amount=that.lister.amount;
                }
               }
             })
           },
        getuserlike:function(){
        var that=this;
    	$.ajax({
        	url:common.apidomain+"/activity/share_wish/likers",
        	dataType: 'json',
        	data:{
           		wuid:that.wuid,
				thirdparty_no:that.thirdparty_no,
	 			thirdparty_type:0
            },
        	success:function(data){
            if(data.code==10000){
            	that.userlist=data.data.activity_thirdparty_user_d_t_o_s;
            	console.log(that.userlist);
            	that.point=that.userlist.length;
      
       

    
                }
               }
             })
           },
           
            getphone:function(){
				var userobj=JSON.parse(window.localStorage.getItem('userphone'));
            	this.userphone= userobj;
			},
			changePhone:function(){ 
        		this.$router.replace({path: '/news/changephone'});
      		},

			}
	
	}

</script>

<style scoped>
.mui-content {

	position: relative;
	width:100%;
	background-repeat: no-repeat;
    background: rgba(26,9,83,1);
	
  /*	padding-top: 100px;*/
}
.c_bgc{
	 padding: 0 1.5rem;
	position: relative;
    padding-top: 5.9rem;
	background-color: #527cff;
	width:100%;
	height: 7.89rem;

  	margin: auto;
}
/*效果*/
.tips_h{
	opacity:0;
	position: absolute;
	left: 0;
    margin: auto;
    right: 0;

   top:-3.81rem;
   z-index: 3;
   transition: all 3s ease-out 3s;
   display: block;
  	animation: tip_down 3s linear forwards;
}
  @keyframes tip_down {
      /*百分比是相对于动画的执行时间*/
      0% {
        transform: translate(0, 0px) ;
        opacity:0.5;
        
      }
      30% {
        transform: translate(0, 16px);
        opacity:0.6;
  
      }
      30% {

        transform: translate(0, 24px);
        opacity:0.7;
  
      }

      45% {

      transition: all 1s ease-out 1s;
           transform: translate(0, 48px);
             opacity:1;
      }
      60% {

         transform: translate(0, 48px);
         opacity:1;
      
      }

      85% {
          transform: translate(0, 24px);
       transition: all 2s ease-out 2s;
       opacity:0.5;
       width: 28rem;
       /* height: 200px;*/
      }
       100% {
        width: 0px;
        height: 0px;
        transform: translate(0, 24px);
       	transition: all 2s ease-out 2s;
      	opacity:0;
      }
    }
.tips_h>img{
	width: 100%;
/*	height: 100%;*/

   
}


.c_bgc .c_input{
	 position: absolute;
   	left: 0;
    margin: auto;
    right: 0;

    height: 3.25rem;
    border-radius: 50%;

      top: -2.5rem;

    height: 9rem;
    padding: 1.5rem;
      opacity:0;
    transition: all 1s ease-out 1s;
  animation: ttip 1s ease-in forwards;
}
 @keyframes ttip{
     0% {

        width:20%;
        opacity:0;
        
      }
       25% {
        width:40%;
        opacity:0.3;
        
      }
       50% {
        width:60%;
        opacity:0.6;
        
      }
       75% {
        width:80%;
        opacity:0.9;
        
      }
       100% {
        width:100%;
        opacity:1;
        
      }
  }
.c_input>img{
	  width: 100%;
    height: 100%;
    padding: 0rem 1rem;

}
.c_input .tip_detil{
	
	    position: absolute;
    top:3.5rem;
    left:3.394rem;
    bottom: 0;
    right: 0;
    margin: auto;
}
.c_input .tip_detil>h3{
	font-size: 1.125rem;
	color:#E84E7A ;
	font-family: PingFangSC;
}
.c_input .tip_detil>p{
	font-size: 0.75rem;
	font-family: PingFangSC;
	color:#9B9B9B;
	margin-top:0.125rem;
}
.c_input .money{
	position: absolute;
    margin: auto;
	font-weight: 700;
	color:#E84E7A ;
	font-size:1.5rem;
    position: absolute;
   right:12%;
    top: 3.94rem;
    margin: auto;
    bottom: 0;
}
.c_input .money>span{
	font-weight: 700;
	font-size:2.5rem;
}
  
.c_bgc>a{

	text-align: center;
	margin: 0 auto;

    width: 19.82rem;
    height: 3.25rem;
	display: block;
	width:100%;
	overflow: hidden;

}
.c_bgc>a>h4{
	color: #fff;
	position: absolute;
	
	left:0;
	bottom: 0;
	right: 0;
	margin: auto;
}
.c_bgc>a>img{
	width:100%;
	height: 100%;
}
.buttom_bgc{
	border-radius:0 0 50% 50%;
	background-color: #527cff;
	width:100%;
	height:4rem;

}
.buttom_bgc .change_s{
	width: 100%;
    margin: 0 auto;
	text-align: center;
    padding-top: 1.6rem;
    

}
.change_s>span{
	font-weight: 700;
	color:#fff;
	  margin: 0 3.2rem 0 1.25rem;
	 font-size: 0.75rem;
}
.change_s>label{
		font-weight: 700;
	color:#fff;
	font-size: 0.75rem;
}
.change_s>a{
	padding-bottom:1px;
	font-weight: 700;
	color:#FECB85;
	font-size: 0.75rem;
	text-decoration:underline;
	
}
  

.footer {

  width: 100%;
  bottom: 0;
  left: 0;
  height:15.6rem;
background:rgba(26,9,83,1);
}
.tip_title>h4{
	margin-left:4%;
	
	color:#fff;
	font-size:0.875rem;
	width: 70%;
    float: left;
	line-height: 1.25rem;
}
.footer .tip_title{
	height: 1.25rem;
	text-align: center;
	padding-top:3.25rem;
	
}
.tip_title .tip_img{
	height: 1.25rem;
    width: 25%;
	 float: left;
}
.tip_img>img{
	background-position: center center;
  	background-repeat:no-repeat;
  	background-size: cover;
  	margin: auto;
  	width:100%;
}
/*效果*/



.footer>p{
	font-size: 0.75rem;
	margin-left:10.5%;
	color:#fff;
	margin-top:1.65rem;

}
.footer>ul{
	margin-top:1.92rem;
	margin-left:10%;
	float:left;
	width:65%;
	 overflow: hidden;
}
.footer>ul>li {
width:2.66rem;

background:rgba(147,184,255,1);
 border-radius: 0.37rem;
height: 2.66rem;
float: left;
list-style: none;
margin-left:4%;
overflow: hidden;

}
.footer>ul>li>img{
width: 100%;
height: 100%;

}
.ridio_d .ridio_li{
	background:rgba(147,184,255,1);
	float: left;
	width:0.63rem;
	height:0.63rem;
	border-radius:50%;
	margin-top: 1rem;
	margin-right: 0.65rem;
}
.ridio_d.ridio_left{

}
.footer .ridio_d{
	margin-top: 1.92rem;
	float:left;
	width: 23%;
    margin-left: 2%;
    overflow: hidden;
}

</style>
