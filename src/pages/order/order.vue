<template>
	<div style="margin: 0 auto; min-height: 620px">
		<div style="height: 30px"></div>
		<div style="text-align:center; margin-top: 20px; background-color: #fff; width: 50%; margin: 0 auto">
			<div style="height: 20px"></div>
			<el-form :inline="true" :model="orderInfo" :rules="rules" ref="orderInfo" class="demo-form-inline" style="width: 100%" label-width="100px">
				<el-form-item label="牛奶类型" prop="mikeType">
					<el-select v-model="orderInfo.mikeType" placeholder="请选择牛奶类型" style="width: 200px">
						<el-option
							v-for="item in mikeTypes"
							:key="item.value"
							:label="item.label"
							:value="item.value">
						</el-option>
					</el-select>  
				</el-form-item>
				<el-form-item label="每天数量" prop="number">
					<el-input-number style="width: 200px" v-model="orderInfo.number" @change="handleChange" :min="1" :max="10" label="每天配送数量"></el-input-number>
				</el-form-item>
			</el-form>
			<el-form :inline="true" :model="orderInfo" :rules="rules" ref="orderInfo" class="demo-form-inline" style="width: 100%" label-width="100px">
				<el-form-item label="开始日期" prop="startDate">
					<div class="block">
						<el-date-picker
							v-model="orderInfo.startDate"
							type="date"
							placeholder="选择日期"
							style="width: 200px">
						</el-date-picker>
					</div>
				</el-form-item>
				<el-form-item label="结束日期" prop="endDate">
					<div class="block">
						<el-date-picker
							v-model="orderInfo.endDate"
							type="date"
							placeholder="选择日期"
							style="width: 200px">
						</el-date-picker>
					</div>
				</el-form-item>
			</el-form>
			<el-form :inline="true" :model="orderInfo" :rules="rules" ref="orderInfo" class="demo-form-inline" style="width: 100%" label-width="100px">
				<el-form-item label="配送间隔" prop="interval">
					<el-select v-model="orderInfo.interval" placeholder="请选择配送间隔" style="width: 512px">
						<el-option
							v-for="item in intervals"
							:key="item.value"
							:label="item.label"
							:value="item.value">
						</el-option>
					</el-select>  
				</el-form-item>
			</el-form>
			<el-form :inline="true" :model="orderInfo" :rules="rules" ref="orderInfo" class="demo-form-inline" style="width: 100%" label-width="100px">
				<el-form-item label="备注信息">
					<el-input type="textarea" v-model="orderInfo.note" placeholder="如有备注信息，请填写" style="width: 512px"></el-input>
				</el-form-item>
			</el-form>
			<el-form :inline="true" :model="orderInfo" :rules="rules" ref="orderInfo" class="demo-form-inline" style="width: 100%" label-width="100px">
				<el-form-item label="配送地址" prop="address">
					<el-input type="textarea" v-model="orderInfo.address" placeholder="请填写联系人、联系方式、配送地址" style="width: 512px"></el-input>
				</el-form-item>
			</el-form>
			<hr style=" height:2px;width: 800px; border:none;border-top:2px dotted #D6D6D6; margin: 0 auto" />
			<br>
			<el-form :inline="true" :model="orderInfo" :rules="rules" ref="orderInfo" class="demo-form-inline" style="width: 100%" label-width="70px">
				<el-form-item >
					<!-- <el-button @click="readonly2=false" style="width: 100px" type="info">修改</el-button> -->
					<el-button style="width: 100px" @click="reserve('orderInfo')" type="success">预定</el-button>
				</el-form-item>
				<el-form-item >
					<!-- <el-button @click="readonly2=false" style="width: 100px" type="info">修改</el-button> -->
					<el-button style="width: 100px" @click="commit2" type="success">返回</el-button>
				</el-form-item>
			</el-form>
			<br>
			
		</div>
	</div>
</template>

<script>
export default {
	name: 'Order',
	data () {
		return {
			orderInfo: {
				mikeType: '',
				number: '',
				startDate: '',
				endDate: '',
				interval: '',	//间隔
				note: '',
				phone: '',
				token: '',
				address: '',
			},
			mikeTypes: [{
				value: "牛奶类型1",
          		label: "牛奶类型1"
			},{
				value: "牛奶类型2",
          		label: "牛奶类型2"
			},{
				value: "牛奶类型3",
          		label: "牛奶类型3"
			},{
				value: "牛奶类型4",
          		label: "牛奶类型4"
			}],
			intervals: [{
				value: 1,
				label: "每天送一次"
			},{
				value: 2,
				label: "两天送一次"
			},{
				value: 3,
				label: "三天送一次"
			}],
			rules: {
				mikeType: [
					{required: true, message: '牛奶类型不能为空', trigger: 'blur'},
				],
				number: [
					{required: true, message: '每天数量不能为空', trigger: 'blur'},
				],
				startDate: [
					{required: true, message: '开始日期不能为空', trigger: 'blur'},
				],
				endDate: [
					{required: true, message: '结束日期不能为空', trigger: 'blur'},
				],
				interval: [
					{required: true, message: '配送间隔不能为空', trigger: 'blur'},
				],
				address: [
					{required: true, message: '配送地址不能为空', trigger: 'blur'},
				],
			},
		}
	},
	created(){
		this.getSession()
	},
	computed: {
    },
	methods: {
		getSession() {
			this.orderInfo.mikeType = sessionStorage.getItem('mikeType')
			this.orderInfo.phone = sessionStorage.getItem('phone')
			this.orderInfo.token = sessionStorage.getItem('token')
		},
		reserve(orderInfo) {
			this.$refs[orderInfo].validate((valid) => {
			  	if (valid) {
			    this.$http.Reserve(this.orderInfo).then((result) => {
			      if (result.c === 200) {
			        this.$message({
			          message: '预定成功！请及时付款',
			          type: 'success'
			        });
			        this.$emit('flushQuery')
			        this.$router.push({name: 'Payment', params:{}})
			      } else {
			        this.$message.warning(result.r)
			      }
			    }, (err) => {
			      this.$message.error(err.msg)
			    })
			  } else {
			    this.$message.warning("请完善信息！")
			  }
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