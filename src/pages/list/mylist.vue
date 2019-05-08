<template>
	<div style="margin: 0 auto; min-height: 600px">
		<div style="margin-top: 20px; margin-left: 120px; height: 64px">
			<!-- <el-form :inline="true" :model="queryData" class="demo-form-inline">
				<el-form-item style="margin-left: 800px">
					<AddEntrPro />
				</el-form-item>
			</el-form> -->
		</div>
		<div style="text-align:center; margin-top: 20px; background-color: #fff; width: 85%; margin: 0 auto">
			<div style="text-align:center; margin-top: 20px; background-color: #fff; width: 90%; margin: 0 auto">
				<div style="height: 10px"></div>
				<el-table :data="myOrder" v-loading="isDisAble">
					<el-table-column prop="mikeType" label="牛奶类型" width="120"></el-table-column>
					<el-table-column prop="number" label="配送数量" width="100"></el-table-column>
					<!-- <el-table-column prop="proIntr" label="项目简介" width="100"></el-table-column> -->
					<el-table-column prop="sendDate" label="配送日期" width="130"></el-table-column>
					<el-table-column prop="address" label="配送地址" width="200"></el-table-column>
					<el-table-column prop="note" label="买家备注" width="120"></el-table-column>
					<el-table-column prop="state" label="订单状态" width="80"></el-table-column>
					<el-table-column prop="price" label="价格" width="80"></el-table-column>
					<el-table-column fixed="right" label="操作" width="140">
						<template slot-scope="scope">
							<!-- <el-button type="text" size="small" @click="showProofMaterial(scope.$index, scope.row)">下载材料</el-button> -->
							<el-button type="text" size="small" @click="deleteOrder(scope.$index, scope.row)" v-if="scope.row.state == '未支付'">删除</el-button>
						</template>
					</el-table-column>
				</el-table>
			</div>
			<br>
		</div>
		
	</div>
</template>

<script>
export default {
	name: 'MyList',
	data () {
		return {
			loading: true,
			myOrder: [
				{
					id: '',
					mikeType: '',
				    number: '',
				    sendDate: '',
				    address: '',
				    note: '',
   					state: '',  // 1-未付款，2-已付款，3-配送中，4-已送达，0-已删除
   					price: '',
				}			
			]
		}
	},
	created(){
		this.queryData()
	},
	computed: {
    },
	methods: {
		queryData(){
			this.isDisAble = true
			this.$http.ShowOrders(sessionStorage.getItem("phone")).then((result) => {
				if (result.c == 200) {
					this.myOrder = result.r
				} else {
					this.myOrder = []
				}
				this.isDisAble = false
			}, (err) => {
	            this.$message.error(err.msg)
	        })
		},
		deleteOrder(index, row){
			this.$confirm('此操作将永久删除该订单, 是否继续?', '提示', {
		       confirmButtonText: '确定',
		       cancelButtonText: '取消',
		       type: 'warning'
		     }).then(() => {
		       this.$http.DeleOrder(row.id).then((result) => {
		         if (result.c === 200) {
		           this.$message({
		             message: '删除成功',
		             type: 'success'
		           });
		           this.$emit('flushQuery')
		           this.queryData()
		         } else {
		           this.$message.warning(result.r)
		         }
		       }, (err) => {
		         this.$message.error(err.msg)
		       })
		     }).catch(() => {
		       this.$message({
		         type: 'info',
		         message: '已取消删除'
		       });          
		     });
		}
		
	}
}		
</script>

<style scoped>
	.labcenter-main{
		margin: 0 auto;
	}
</style>