<template>
	<div id="app1">
		<div class="back">
			<!--<img src="../../common/img/sdk/close.png" @click="backLogin()" />-->
		</div>
		<div class="logo">
			<img src="../../common/img/sdk/logo.png" />
		</div>
		<div class="form">
			<div class="msg">
				<div class="username">
					<img src="../../common/img/sdk/login_user.png" />
					<input class="form_input" type="text" name="username" id="username" value="" placeholder="用户名" />
				</div>
				<div class="password">
					<img src="../../common/img/sdk/login_pwd.png" />
					<input class="form_input" type="password" name="password" id="password" value="" placeholder="密码" />
				</div>
			</div>

			<input class="submit" type="button" @click="log_submit" value="登录" />
			<div class="yhxy">
				<span class="yhxys" @click="changePage(1)">注册账号</span>
				<span class="findPwd" @click="changePage(2)">忘记密码?</span>
			</div>
		</div>
		
		<div class="googleVen" v-show="isshow">
			<div class="title">
				<span class="name">输入谷歌验证码</span>
				<span class="cannel" @click="log_submit">取消</span>
			</div>
			<div class="input">
				<input type="text" class="input-juli" maxlength="1" placeholder="-" />
				<input type="text" class="input-juli" maxlength="1" placeholder="-" />
				<input type="text" class="input-juli" maxlength="1" placeholder="-" />
				<input type="text" class="input-juli" maxlength="1" placeholder="-" />
				<input type="text" class="input-juli" maxlength="1" placeholder="-" />
				<input type="text" class="input-juli" maxlength="1" placeholder="-" />
			</div>
		</div>
		

	</div>
</template>

<script>
	export default {
		data: function() {
			return {
				loginData: "",
				updateData:"",
				isshow: false,
				
			}
		},
		methods: {
			showQrCheck: function(code) {
				let vm = this;
				$.ajax({
					type: "post",
					url: contextPath + "/liyu_game/api/googleauthenticator/validation",
					async: true,
					dataType: "json",
					data: {
						token: vm.loginData.accessToken,
						code:code
					},
					success: function(arg) {
						vm.callBack();
					}
				});
			},
			back: function() {
				back();
			},
			changePage: function(id) {
				let vm = this;
				vm.$emit('changePage', id);
			},
			callBack: function() {
				let vm = this;
							window.localStorage.token =vm.loginData.accessToken;
								window.localStorage.openId = vm.loginData.openId;
								window.localStorage.isLogin = "1";
				
				localStorage.userData = JSON.stringify(vm.updateData);
				var ua = window.navigator.userAgent.toLowerCase();
				if(ua.match(/MicroMessenger/i) == 'micromessenger') {
					location.href = "http://www.liyugame.com/myAPI/view/wxauthorization/wx_open_authorization?redirect_url=" + encodeURIComponent(vm.bakurl) + "&token=" + localStorage.token;
				} else {
					location.href = vm.bakurl;
				}
			},
			log_submit: function() {
				let vm = this;
				
				var username = $("#username").val();
				var password = $("#password").val();

				if(username == '') {
					mui.toast("请输入用户名");
				} else if(password == '') {
					mui.toast("请输入密码");
				} else {
					$.ajax({
						url: contextPath + "/liyu_game/api/h5sdk/sdklogin",
						type: "post",
						async: true,
						dataType: "json",
						data: {
							username: username,
							pwd: password
						},
						success: function(data) {
							var code = data.state.code;
							if(code == '20000') {
								vm.loginData=data.data;
								if(data.data.isGoogleAuthenticator==1){
									$.ajax({
										type: "get",
										url: contextPath + "/liyu_game/api/h5sdk/getUserById",
										async: true,
										data: {
											token: data.data.accessToken
										},
										dataType: "json",
										success: function(updateData) {
											vm.updateData=updateData.data;
											vm.isshow = !vm.isshow;
										}
									});
								}else{
									$.ajax({
										type: "get",
										url: contextPath + "/liyu_game/api/h5sdk/getUserById",
										async: true,
										data: {
											token: data.data.accessToken
										},
										dataType: "json",
										success: function(updateData) {
											vm.updateData=updateData.data;
											vm.callBack();
										}
									});
								}
								

							} else {
								mui.toast("用户名或密码错误");
								form1.password.value = "";
							}
						}
					});
				}
				return false;
			}
		},
		mounted: function() {
			let vm = this;
			$('.input-juli').on('keyup', function(e) {
				if(e.keyCode==46){
					var $input = $(this).prev('input');
					if($input.length > 0 && $(this).val()=="") {
						$input.focus();
						$($input).val("");	
					}else{
						$(this).val("");					
					}
				}else{
					var $input = $(this).next('input');
					if($input.length > 0) {
						$input.focus();
					} else {
						var numStr = "";
						var inputjulis = document.querySelectorAll(".input-juli");
						for(var i = 0; i < inputjulis.length; i++) {
							numStr = numStr + $(inputjulis[i]).val();
						}
						vm.showQrCheck(numStr);
					}
				} 
				
				
			});
		},
		props: ['bakurl']
	}
</script>

<style lang="less" scoped>
	#app1 {
		.forget {
			display: block;
			height: 0.58rem;
			width: 100%;
			color: #909090;
			text-align: left;
			line-height: 0.58rem;
			-webkit-transform: scale(0.75) translateX(-0.8rem);
		}
		.yhxy {
			position: absolute;
			color: #909090;
			font-size: 0.24rem;
			-webkit-transform: scale(0.91);
			width: 100%;
			margin-top: 0.15rem;
		}
		.yhxy {
			.yhxys {
				float: left;
				color: #006b8d;
			}
			.findPwd {
				float: right;
				color: #006b8d;
			}
		}
		.findPwd {
			float: right;
		}
	}
	.googleVen{
		width: 6.4rem;
		height: 100%;
		background: #eee;
		position: fixed;
		top: 0rem;
		left: 0;
	}
	.title{
		width: 6.4rem;
		height: 0.88rem;
		background: #006B8D;
	}
	.cannel{
		width: 1rem;
		height: 0.88rem;
		line-height: 0.88rem;
		color: #fff;
		float: right;
		text-align: center;
	}
	.name{
		height: 0.88rem;
		width: 4.4rem;
		text-align: center;
		margin-left: 1rem;
		line-height: 0.88rem;	
		float: left;
		color: #fff;
	}
	.input {
		width: 5.6rem;
		margin: 0 auto;
		margin-top: 0.25rem;
	}
	
	.input input {
		width: 0.76rem !important;
		height: 0.55rem !important;
		float: left;
		border: none;
		border-bottom: 1px solid #cfcfcf;
		color: #006b8d;
		font-size: 0.34rem;
		margin-left: 0.14rem !important;
		text-align: center;
		border-radius: 0;
		background: #eee !important;
	}
	
</style>