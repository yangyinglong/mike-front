<template>
	<div class="index-wrap">
		<div class="index-left">
			<div class="index-left-block">
				<h2>全部产品</h2>
				<div v-for="(product,index) in productList">
					<h3>{{ product.category }}</h3>
					<ul>
						<li v-for="item in product.list">
							<a target="_blank" :href="item.url">{{ item.name }}</a>
							<span v-if="item.hot" class="hot-tag">HOT</span>					
						</li>
					</ul>
					<div v-if="index%2 == 0" class="hr"></div>
				</div>
			</div>
			<div class="index-left-block lastest-news">
				<h2>小店公告</h2>
				<ul>
					<li>最新开业，请多多光顾，新老顾客，优惠多多</li>
				</ul>
			</div>
		</div>
		<div class="index-right">
			<div class="swiper-size">
				<swiper :options="swiperOption">
				   	<swiper-slide>
				   		<a target="_blank" href="http://www.wwtliu.com/blog">
				   			<img class="swiperimg" src="../assets/slideShow/1.jpg" alt="">
				   		</a>
				   	</swiper-slide>
				   	<swiper-slide>
				   		<a target="_blank" href="http://www.wwtliu.com/blog">
				   			<img class="swiperimg" src="../assets/slideShow/2.jpg" alt="">
				   		</a>
				   	</swiper-slide>
				   	<swiper-slide>
				   		<a target="_blank" href="http://www.wwtliu.com/blog">
				   			<img class="swiperimg" src="../assets/slideShow/3.jpg" alt="">
				   		</a>
				   	</swiper-slide>
				   	<swiper-slide>
				   		<a target="_blank" href="http://www.wwtliu.com/blog">
				   			<img class="swiperimg" src="../assets/slideShow/4.jpg" alt="">
				   		</a>
				   	</swiper-slide>
				   	<div class="swiper-pagination"  slot="pagination"></div>
				   	<div class="swiper-button-prev" slot="button-prev"></div>
				   	<div class="swiper-button-next" slot="button-next"></div>
				</swiper>
			</div>
			<div class="index-board-list">
				<div class="index-board-item" v-for="(borad, index) in boradList" :class="['index-board-' + borad.tag, {'line-last' : index%2 !== 0}]">
					<div class="index-board-item-inner">
						<h2>{{ borad.title }}</h2>
						<p>{{ borad.desc }}</p>
						<div class="index-board-button">
							<lable @click="order(borad.title)" class="button">立即预定</lable>
							<!-- <el-button @click="order" type="text">立即预定</el-button> -->
							<!-- <router-link :to="{path:'/order'}" class="button">立即预定</router-link> -->
						</div>
					</div>					
				</div>
			</div>
		</div>
	</div>
</template>

<script>

export default{
	name:"layout",
	data(){
		return{
			swiperOption: {
				pagination: {
				    el: '.swiper-pagination'
				},
				loop:true,
				autoplay:true,
				navigation: {
					nextEl: '.swiper-button-next',
					prevEl: '.swiper-button-prev',
			    }
			},
			news:[],
			boradList:[
				{
					title:"牛奶类型1",
					desc:"这是一款纯牛奶，让您更健康",
					tag:"type1"
				},
				{
					title:"牛奶类型2",
					desc:"这是一款纯牛奶，让您更健康",
					tag:"type2"
				},
				{
					title:"牛奶类型3",
					desc:"这是一款纯牛奶，让您更健康",
					tag:"type3"
				},
				{
					title:"牛奶类型4",
					desc:"这是一款纯牛奶，让您更健康",
					tag:"type4"
				}
			],
			productList:[
				{
					category:"全部产品",
					list:[
						{
							name:"牛奶类型1",
							url:"http://www.wwtliu.com/blog",
							hot:false
						},
						{
							name:"牛奶类型2",
							url:"http://www.wwtliu.com/blog",
							hot:true
						},
						{
							name:"牛奶类型3",
							url:"http://www.wwtliu.com/blog",
							hot:false
						},
						{
							name:"牛奶类型4",
							url:"http://www.wwtliu.com/blog",
							hot:false
						}
					]
				},
				{
					category:"优惠活动",
					list:[
						{
							name:"优惠活动1",
							url:"http://www.wwtliu.com/blog",
							hot:true
						},
						{
							name:"优惠活动2",
							url:"http://www.wwtliu.com/blog",
							hot:false
						},
						{
							name:"优惠活动3",
							url:"http://www.wwtliu.com/blog",
							hot:true
						},
						{
							name:"优惠活动4",
							url:"http://www.wwtliu.com/blog",
							hot:false
						}
					]
				}
			]

		}
	},
	methods: {
		order(title) {
			if (this.$store.state.loginStatus == 0) {
				this.$notify.info({
					title: '请登录',
					message: '亲！登录之后，预定查看更方便哟'
				});
			}else {
				sessionStorage.setItem('mikeType', title)
				this.$router.push({name: 'Order', params: {}})
			}
		}
	},
	created() {
		// this.$axios.get("http://www.wwtliu.com/sxtstu/news/juhenews.php", {
		// 	params:{
		// 		count: 10,
		// 		type: "top"
		// 	}
		// })
		// .then(res => {
		// 	console.log(res.data);
		// 	this.news = res.data;
		// })
		// .catch(error => {
		// 	console.log(error)
		// })
	}
}
</script>

<style scoped>

.index-wrap {
	width: 1200px;
	margin: 0 auto;
	overflow: hidden;

}
.index-left {
	float: left;
	width: 300px;
	text-align: left;
}
.index-right {
	float: left;
	width: 900px;
}
.index-left-block {
	margin: 25px;
	background: #fff;
	box-shadow: 0 0 10px #ddd;
	height: 450px;
}
.index-left-block .hr {
	margin-bottom: 10px;
	height: 2px;
	width: 100%;
	background: #ddd;
}
.index-left-block h2 {
	background: #4fc08d;
	color: #fff;
	padding: 10px 15px;
	margin-bottom: 20px;
}
.index-left-block h3 {
	padding: 0 15px 5px 15px;
	font-weight: bold;
	color: #222;
}
.index-left-block ul {
	padding: 0px 7px;
}
.index-left-block li {
	list-style: none;
	padding: 5px;
}
.index-left-block a {
	color: #222;
	text-decoration:none;
	font-size: 15px;
}
.index-board-list {
	overflow: hidden;
	margin-top: 20px;
}
.index-board-item {
	float: left;
	width: 440px;
	background: #fff;
	box-shadow: 0 0 1px #ddd;
	padding: 20px;
	margin-right: 20px;
	margin-bottom: 20px;
}
.index-board-item-inner {
	min-height: 125px;
	padding-left: 120px;
}
.index-board-type1 .index-board-item-inner {
	background: url(../assets/images/1.jpg) no-repeat;
}
.index-board-type2 .index-board-item-inner {
	background: url(../assets/images/2.jpg) no-repeat;
}
.index-board-type3 .index-board-item-inner {
	background: url(../assets/images/3.jpg) no-repeat;
}
.index-board-type4 .index-board-item-inner {
	background: url(../assets/images/4.jpg) no-repeat;
}
.index-board-item h2 {
	font-size: 18px;
	font-weight: bold;
	color: #000;
	margin-bottom: 15px;
}
.line-last {
	margin-right: 0;
}
.index-board-button {
	margin-top: 20px;
}
.lastest-news {
	/*min-height: 370px;*/
	height: 349px;
}
.hot-tag {
	background: red;
	color: #fff;
}
.new-item {
	display: inline-block;
	width: 230px;
	overflow: hidden;
	text-overflow: ellipsis;
	white-space: nowrap;
}
.swiperimg {
	width: 100%;
	height: 450px;
	margin-top: 25px;
}
.wiper-size {
	margin-top: 20px;
}
.button {
	background: #4fc08d;
	color: #fff;
	/*display: inline-block;*/
	padding: 10px 20px;
	cursor: pointer;
	text-decoration:none;
}
</style>