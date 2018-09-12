<template>
	<div class="pos">
		<div>
			<el-row>
				<el-col :span='7' id="order-list">
				
					<el-tabs>
						<el-tab-pane label="点餐"></el-tab-pane>
						<el-tab-pane label="点单"></el-tab-pane>
						<el-tab-pane label="外卖"></el-tab-pane>
					</el-tabs>

					<el-table :data="tableData" border show-summary style="width: 100%">
						<el-table-column prop="goodsName" label="商品"  ></el-table-column>
    					<el-table-column prop="count" label="量" width="50"></el-table-column>
    					<el-table-column prop="price" label="金额" width="70"></el-table-column>
   						<el-table-column  label="操作" width="100" fixed="right">
					        <template slot-scope="scope">
					            <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
					            <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
					 
					        </template>
					 </el-table-column>
					 

					</el-table>
					<div class="totalDiv">
					 	<small>数量</small>{{totalCount}}&nbsp;&nbsp;&nbsp;&nbsp;<small>金额</small> {{totalMoney}}元
					 </div>
					<el-button type="warning" >挂单</el-button>
							<el-button type="danger" @click=" delAllGoods()">删除</el-button>
							<el-button type="success" @click="checkout">结账</el-button>

					
				</el-col>
				<el-col :span='17' id="order-list">

					<div class="often-goods">
							<div class="title">常用商品</div>
								<div class="often-goods-list">
									<ul class="often-goods">
										<li v-for="goods in oftenGoods" @click="addOrderList(goods)">
			
												<span>{{goods.goodsName}}</span>
												<span calss="o-price">
												￥{{goods.price}}元
													</span>
										</li>
									</ul>
										
									

							</div>
									
					</div>
					<div class="goods-type">
 
					    <el-tabs>
					        <el-tab-pane label="汉堡">
								<ul class='cookList'>
									<li v-for="goods in type0Goods" @click="addOrderList(goods)">
									    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
									    <span class="foodName">{{goods.goodsName}}</span>
									    <span class="foodPrice">￥{{goods.price}}元</span>
									</li>
								</ul>
					        </el-tab-pane>
					        <el-tab-pane label="小食">
					            <ul class='cookList'>
									<li v-for="goods in type1Goods" @click="addOrderList(goods)">
									    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
									    <span class="foodName">{{goods.goodsName}}</span>
									    <span class="foodPrice">￥{{goods.price}}元</span>
									</li>
								</ul>
					        </el-tab-pane>
					        <el-tab-pane label="饮料">
					            <ul class='cookList'>
									<li v-for="goods in type2Goods" @click="addOrderList(goods)">
									    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
									    <span class="foodName">{{goods.goodsName}}</span>
									    <span class="foodPrice">￥{{goods.price}}元</span>
									</li>
								</ul>
					        </el-tab-pane>
					        <el-tab-pane label="套餐">
					           <ul class='cookList'>
									<li v-for="goods in type3Goods" @click="addOrderList(goods)">
									    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
									    <span class="foodName">{{goods.goodsName}}</span>
									    <span class="foodPrice">￥{{goods.price}}元</span>
									</li>
								</ul>
					        </el-tab-pane>
					 
					    </el-tabs>
					</div>


					
				</el-col>
			</el-row>
		</div>
		<div>
			<el-button @click="show=!show">Click Me!</el-button>

			<div style="display:flex;margin-top:100px;height:100px;">
				<transition name="el-fade-in-linear">
					<div v-show="show" class="transition-box">.el-fade-in-linear</div>
				</transition>
				<transition name="el-fade-in">
					<div v-show="show" class="transition-box">.el-fade-in</div>
				</transition>


			</div>
		</div>

		<div>
			<el-button @click="show2=!show2">Click Me!</el-button>
			
			<div style="display:flex;margin-top:100px;height:100px;">
				<transition name="el-zoom-in-center">
					<div v-show="show2" class="transition-box">.el-zoom-in-center</div>
				</transition>
				<transition name="el-zoom-in-top">
					<div v-show="show2" class="transition-box">.el-zom-in-top</div>
				</transition>
				<transition name="el-zoom-in-bottom">
					<div v-show="show2" class="transition-box">.el-zoom-in-bottom</div>
				</transition>


			</div>
		</div>

		
	


	</div>
</template>
<script>
	import axios from 'axios';
	
	export default{
			name:'Pos',
			data(){				
					return {
					tableData:[],
					oftenGoods:[],
			        type0Goods:[],
			        type1Goods:[],
			        type2Goods:[],
			        type3Goods:[],
			        totalMoney:0,
			        totalCount:0,
			        show:true,
			        show2:true
        			
        		}

			},
			



			created:function(){
		      axios.get('http://jspang.com/DemoApi/oftenGoods.php')
		      .then(response=>{
		         console.log(response);
		         this.oftenGoods=response.data;
		      })
		      .catch(error=>{
		          console.log(error);
		          alert('网络错误，不能访问');
		      });
		      axios.get('http://jspang.com/DemoApi/typeGoods.php')
		      .then(response=>{
		         console.log(response);
		         //this.oftenGoods=response.data;
		         this.type0Goods=response.data[0];
		         this.type1Goods=response.data[1];
		         this.type2Goods=response.data[2];
		         this.type3Goods=response.data[3];
		 
		      })
		      .catch(error=>{
		          console.log(error);
		          alert('网络错误，不能访问');
		      })
		  },
		

			mounted:function(){
					var orderHeight=document.body.clientHeight;
					document.getElementById("order-list").style.height=orderHeight+'px';
			},


			methods:{
      //添加订单列表的方法
		      addOrderList(goods){
		            this.totalCount=0; //汇总数量清0
		            this.totalMoney=0;
		            let isHave=false;
		            //判断是否这个商品已经存在于订单列表
		            for (let i=0; i<this.tableData.length;i++){
		                console.log(this.tableData[i].goodsId);
		                if(this.tableData[i].goodsId==goods.goodsId){
		                    isHave=true; //存在
		                }
		            }
		            //根据isHave的值判断订单列表中是否已经有此商品
		            if(isHave){
		                //存在就进行数量添加
		           
		                 let arr = this.tableData.filter(o =>o.goodsId == goods.goodsId);
		                 arr[0].count++;
		                 //console.log(arr);
		            }else{
		                //不存在就推入数组
		                let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
		                 this.tableData.push(newGoods);
		 
		            }
		 
		            //进行数量和价格的汇总计算
		            this.tableData.forEach((element) => {
		                this.totalCount+=element.count;
		                this.totalMoney=this.totalMoney+(element.price*element.count);   
		            });
		      },
		      delSingleGoods(goods){
		      		console.log(goods);
		      		this.tableData=this.tableData.filter(o=>o.goodsId!=goods.goodsId)
		      },
		      delAllGoods(){
			      	this.tableData=[];
			      	this.totalCount=0;
			      	this.totalMoney=0;
		      },
		      checkout(){
		      		if(this.totalCount!=0){
		      			this.totalData=[];
		      			this.totalCount=0;
		      			this.totalMoney=0;
		      			this.$message({
		      				message:'结账成功',
		      				type:'success'
		      			});
		      			}
		      			else{
		      				this.$message.error('不能空结');
		      			}
		      		}
		      
		  }
	}
</script>
<style scoped>
.title{
       height: 20px;
       border-bottom:1px solid #D3DCE6;
       background-color: #F9FAFC;
       padding:10px;
   }
   .often-goods-list ul li{
      list-style: none;
      float:left;
      border:1px solid #E5E9F2;
      padding:10px;
      margin:5px;
      background-color:#fff;
   }
  .o-price{
      color:#58B7FF; 
   }
   .goods-type{
   margin-top:20px;
   }
   .goods-type{
   clear:both;
   }
 .cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auto;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;

 
   }
   .cookList li span{
       	float:left;
        display: block;
        
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;
 
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
   .transition-box {
    margin-bottom: 10px;
    width: 200px;
    height: 100px;
    border-radius: 4px;
    background-color: #409EFF;
    text-align: center;
    color: #fff;
    padding: 40px 20px;
    box-sizing: border-box;
    margin-right: 20px;
  }

</style>