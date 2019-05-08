<template>
	<div style="margin: 0 auto; min-height: 620px">
		<div style="height: 30px"></div>
		<div style="text-align:center; margin-top: 20px; background-color: #fff; width: 50%; margin: 0 auto">
			<div style="height: 20px"></div>
			<img src="@/assets/wechatPay.png" style="height: 300px; margin-right: 30px">
			<img src="@/assets/alipay.jpg" style="height: 300px; width: 212px">
			<br><br><br>
			<label style="font-size: 16px">应该支付 {{money}} 元</label>
			<br><br><br>
			<el-form :inline="true" class="demo-form-inline" style="width: 100%" label-width="70px">
				<el-form-item >
					<!-- <el-button @click="readonly2=false" style="width: 100px" type="info">修改</el-button> -->
					<el-button style="width: 120px" @click="hadPay" type="success">通知已付款</el-button>
				</el-form-item>
				<el-form-item >
					<!-- <el-button @click="readonly2=false" style="width: 100px" type="info">修改</el-button> -->
					<el-button style="width: 100px" @click="commit2" type="success">返回</el-button>
				</el-form-item>
			</el-form>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Payment',
	data () {
		return {
			money: '0',
		}
	},
	created(){
		this.queryMoney()
	},
	computed: {
    },
	methods: {
		queryMoney() {
			// var info = {
			// 	phone: sessionStorage.getItem('phone'),
			// 	token: sessionStorage.getItem('token')
			// }
			this.$http.QueryMoney(sessionStorage.getItem('phone')).then((result) => {
				if (result.c === 200) {
					this.money = result.r
				} else {
					this.$message.warning(result.r)
				}
			}, (err) => {
				this.$message.error(err.msg)
			})
		},
		hadPay(){
			var form = {
				phone: sessionStorage.getItem('phone'),
				amount: this.money
			}
			this.$http.NoticePayment(form).then((result) => {
				if (result.c === 200) {
					this.$message({
			          message: '请稍后，店家正在查看',
			          type: 'success'
			        });
			        this.$emit('flushQuery')
			        this.$router.push({name: 'MyList', params:{}})
				} else {
					this.$message.warning(result.r)
				}
			}, (err) => {
				this.$message.error(err.msg)
			})
		},
		commit2() {
			this.$router.push({name: 'Layout', params:{}})
		}		
		
	},
	components: {
	},
}		
</script>

<style scoped>
	.labcenter-main{
		width: 70%;
		margin: 0 auto;
	}
</style>