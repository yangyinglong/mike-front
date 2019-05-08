<!-- login.vue -->
<template>
	<div>
		<div v-if="loginStatus === 0">
			<b class="linked" @click="dialogFormVisible = true">{{userName}}</b>
			<el-dialog width="20%" title="登录" :visible.sync="dialogFormVisible" center>
				<div>
					<el-form :model="loginform" ref="loginform" class="demo-ruleForm">
					  <el-form-item>
					    <el-input v-model="loginform.phone" autocomplete="off" placeholder="用户名或手机号码" clearable></el-input>
					  </el-form-item>
					  <el-form-item>
					    	<el-input type="password" v-model="loginform.pass" autocomplete="off" placeholder="用户名或手机号码" clearable></el-input>
					  </el-form-item>
					</el-form>
				</div>

				<div style="width: 200px; margin:0 auto">
				    <div style="float: left">
				        <el-checkbox v-model="loginform.rememberPassword">记住密码</el-checkbox>
				    </div>
				    <router-link to="/forgetPass" >
				        <div style="float: right" @click="dialogFormVisible = false">忘记密码</div>
				    </router-link>
				</div>
			  
			  	<div style="height: 50px"></div>
			  	<div slot="footer" class="dialog-footer">
			  	  	<!-- <el-button @click="dialogFormVisible = false">取 消</el-button> -->
			  	  	<el-button type="primary" style="width: 70%" @click="loginData">确 定</el-button>
			  	</div>
			</el-dialog>
		</div>
		<div v-else>
			<router-link to="/usercenter" style="text-decoration: none;"><b class="linked">{{userName}}</b></router-link>
		</div>
		
	</div>
</template>

<script>

export default {
	name: 'Login',
	data() {
		return {
			dialogFormVisible: false,
			loginform: {
				phone: '',
				name: '',
				pass: '',
				rememberPassword: false
			},
		}
	},
	computed: {
      userName() {
        return this.$store.state.userName
      },
      loginStatus() {
        return this.$store.state.loginStatus
      }
    },
    mounted() {
      this.getCookie()
    },
	methods: {
		loginData: function() {
			var loginFormData = {
				phone: this.loginform.phone,
				password: this.loginform.pass,
				state: 1
			}
			this.$http.Login(loginFormData).then((result) => {
				if (result.c === 200) {
					this.$notify({
					  	type: 'success',
					  	message: '欢迎您，' + result.r.name + '!',
					  	duration: 2000
					})
					sessionStorage.setItem('userId', result.r.stuId)
	                sessionStorage.setItem('userName', result.r.name)
	                sessionStorage.setItem('phone', result.r.phone)
	                sessionStorage.setItem('status', result.r.state)
	                sessionStorage.setItem('token', result.r.token)
	                // this.$emit("changeStatus", result.r.status)
	                this.$store.dispatch('login', result.r).then(() => {
	                  this.$message({
	                    message: '欢迎您，' + result.r.name + '!',
	                    type: 'success'
	                  });
	                })
	                this.setCookie(this.loginform.phone, this.loginform.password, 7, this.loginform.rememberPassword)
	                this.$store.state.userName = result.r.name
	                this.$store.state.loginStatus = result.r.state
	                this.$store.state.token = result.r.token
	                this.$store.state.phone = result.r.phone
	                this.dialogLoginVisible = false
	                this.$router.push({name: 'Layout', params: {}})
				}
				else if (result.c === 401) {
					this.$notify({
					  	type: 'error',
					  	message: '密码错误!',
					  	duration: 2000
					})
				}
				else {
					this.$notify({
					  	type: 'error',
					  	message: '没有此用户!',
					  	duration: 2000
					})
					this.dialogFormVisible = false
				}
				
			})
		},
		clearPass(){
        this.loginform.password = ''
      },
      setCookie(c_name, c_pwd, exdays, remeberFlag) {
        var exdate = new Date(); //获取时间
        exdate.setTime(exdate.getTime() + 24 * 60 * 60 * 1000 * exdays); //保存的天数
        //字符串拼接cookie
        window.document.cookie = "userName" + "=" + c_name + ";path=/;expires=" + exdate.toGMTString();
        window.document.cookie = "userPwd" + "=" + c_pwd + ";path=/;expires=" + exdate.toGMTString();
        window.document.cookie = "remeberFlag" + "=" + remeberFlag + ";path=/;expires=" + exdate.toGMTString();
      }, 
      //读取cookie
      getCookie: function() {
          if (document.cookie.length > 0) {
              var arr = document.cookie.split('; '); //这里显示的格式需要切割一下自己可输出看下
              for (var i = 0; i < arr.length; i++) {
                  var arr2 = arr[i].split('='); //再次切割
                  //判断查找相对应的值
                  if (arr2[0] === 'userName') {
                      this.loginform.phone = arr2[1]; //保存到保存数据的地方
                  } else if (arr2[0] === 'userPwd') {
                      this.loginform.password = arr2[1];
                  } else if (arr2[0] === 'remeberFlag') {
                    if (arr2[1] === "true") {                    
                      this.loginform.rememberPassword = true;
                    } else {
                      this.loginform.rememberPassword = false;
                    }
                  }
              }
          }
        }
	}
}
</script>

<style scoped>
.content {
	display: inline-block;
	text-align: center;
	vertical-align: middle;
	horiz-align: center;
	white-space: nowrap;
	margin: 0;
}
</style>