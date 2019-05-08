<template>
	<div class="app-head">
		<div class="app-head-inner">
			<router-link to="/"><img src="../assets/logo.png" alt=""></router-link>			
			<span style="font-size: 15px">小邵牛奶店：在线预定 | 准时送达 | 价格优惠 | 种类齐全 | 让您的生活更健康</span>
			<div class="head-nav">
				<ul class="nav-list">
					<li>
						<Login />
					</li>
					<li class="nav-pile">|</li>
					<li v-if="loginStatus===0">
						<router-link to="/register">注册</router-link>
					</li>
					<li v-else>
						<router-link :to="{path:'/mylist'}">我的订单</router-link>
					</li>
					<li class="nav-pile">|</li>
					<li>关于</li>
				</ul>
			</div>

		</div>
	</div>
</template>

<script>

import Login from "./user/login"
export default{
	name:"header",
	data(){
		return{

		}
	},
	components: {
		Login
	},
	computed: {
		loginStatus() {
			return this.$store.state.loginStatus
		}
	},
	created(){
		this.getNameFromSession()
	},
	methods: {
		getNameFromSession() {
			if (sessionStorage.getItem('status') == '1' || sessionStorage.getItem('status') == '2') {
				this.$store.state.userName = sessionStorage.getItem('userName')
				this.$store.state.loginStatus = parseInt(sessionStorage.getItem('status'))
				this.$store.state.token = sessionStorage.getItem('token')
				this.$store.state.phone = sessionStorage.getItem('phone')
			} else {
				this.$store.state.userName = '登录',
				this.$store.state.loginStatus = 0,
				this.Login.status = 0
			}
		},
	}
}
</script>
<style scoped>

.app-head {
	background: #363636;
	color: #b2b2b2;
	height: 90px;
	line-height: 90px;
	width: 100%;
	min-width: 1400px;
}
.app-head-inner {
	width: 1200px;
	margin: 0 auto;
}
.app-head-inner span{
	position: relative;;
	top: -20px;
}
.app-head-inner img {
	width: 50px;
	margin-top: 20px;
}
.head-nav {
	float: right;
}
.head-nav ul {
	overflow: hidden;
}
.head-nav li {
	cursor: pointer;
	float: left;
	list-style: none;
}
.nav-pile {
	padding: 0 10px;
}
.head-logo {
	float: left;
}
</style>