<template>
	<div id="con">
		<div class="header">
			<a href="javascript:history.go(-1)"><img src="../../common/img/userCenter/arrowsb.png" alt="" class="back" /></a>
			<span>挖矿贵族卡详情</span>
		</div>
		
		
		<div class="main">
			<div class="main-box">
				<h1>特权说明-“蛋”生的世界</h1>
				<ul>
					<li class="clear"><span>①</span><span>“蛋”生的世界挖矿贵族可享受尊贵游戏特权，帮助获取更高的TTC收益。</span> </li>
					<li class="clear"><span>②</span><span>“蛋”生的世界挖矿贵族卡可在集市交易中心出售。</span></li>
					<li>使用须知：此卡的最终解释权归TTC国际所有。</li>
				</ul>
			</div>

		</div>

		<div class="brood">
			<img src="../../common/img/wa.png" alt="" />
			<p>快来加入“蛋”生的世界，“孵”你的蛋吧！</p>
		</div>

		<div class="btn">
				<button class="btnA" @click="OpenVipCard(cardId)">开启</button>
			<button class="btnB"  @click="sellCard(cardId)">出售</button>
		</div>
	</div>
</template>
<script>
	export default {
		data() {
			return {
				cardId: this.$route.query.cardId
			}
		},
		methods: {
			sellCard:function(id){
				let vm = this;
				mui.prompt("确认后不可撤销","价格","售卖当前道具",["取消","确认"],function(e){
					if(e.index==1){
							$.ajax({
								type: "get",
								url: contextPath + "/liyu_game/api/userInfo/SellCard",
								async: true,
								data: {
									price:e.value,
									cid: id,
									token: localStorage.token
								},
								dataType: "json",
								success: function(data) {
									mui.alert("挂售成功,请在我的交易中查看进度");
								vm.$router.push({"path":"/card"})
								}
							});
					}
				})
				
			},
			OpenVipCard: function(id) {
				debugger
				let vm = this;
				$.ajax({
					type: "get",
					url: contextPath + "/liyu_game/api/userInfo/UseCard",
					async: true,
					data: {
						cid: id,
						token: localStorage.token
					},
					dataType: "json",
					success: function(data) {
						vm.$router.push({
							"path": "/aristocraticcardcon",
						})
					}
				});
			}
		},
		mounted: function() {
			var cona = document.getElementById("con")
			var H = document.documentElement.clientHeight;
			cona.style.height = H + "px";
			cona.style.backgroundColor = "#eee";
		}
	}
</script>

<style scoped>
	.mint-header {
		background: #006b8d;
		height: 0.88rem;
	}
	
	.main {
		width: 6rem;
		height: 2.35rem;
		background: #fff;
		border-radius: 0.06rem;
		margin: 0 auto;
		margin-top: 0.88rem;
		border-top: 0.2rem solid #eee ;
		
	}
	
	.main-box {
		width: 5.6rem;
		height: 2.35rem;
		margin: 0 auto;
		margin-top: 0.2rem;
		overflow: hidden;
	}
	
	.main h1 {
		color: #010101;
		font-size: 0.24rem;
		margin-top: 0.2rem;
	}
	
	.main-box ul li span {
		display: block;
		color: #333;
		font-size: 0.22rem;
		float: left;
	}
	
	.main-box ul li span:nth-of-type(1) {
		width: 0.3rem;
	}
	
	.main-box ul li span:nth-of-type(2) {
		width: 5.3rem;
	}
	
	.main-box ul li {
		color: #666;
		font-size: 0.22rem;
		line-height: 0.45rem;
	}
	
	.brood {
		width: 6rem;
		height: 4.08rem;
		background: #fff;
		margin: 0 auto;
		margin-top: 0.2rem;
		border-radius: 0.06rem;
		overflow: hidden;
	}
	
	.brood img {
		width: 5.42rem;
		height: 3.15rem;
		margin: 0 auto;
		display: block;
		margin-top: 0.3rem;
	}
	
	.brood p {
		font-size: 0.22rem;
		color: #333;
		line-height: 0.55rem;
		margin-left: 0.3rem;
	}
	
	.btn {
		width: 6rem;
		height: 0.6rem;
		margin: 0 auto;
		margin-top: 0.2rem;
	}
	
	.btnA {
		width: 2.9rem;
		float: left;
		background: #008d77;
		color: #fff;
	}
	
	.btnB {
		width: 2.9rem;
		float: right;
		background: #ed8d38;
		color: #fff;
	}
		.header {
		width: 6.4rem;
		height: 0.88rem;
		background: #006b8d;
		position: fixed;
		top: 0;
		z-index: 9999;
	}
	
	.header img {
		width: 0.2rem;
		height: 0.35rem;
		margin-left: 0.2rem;
		float: left;
		margin-top: 0.25rem;
	}
	
	.header span {
		width: 5.6rem;
		color: #fff;
		display: block;
		float: left;
		line-height: 0.88rem;
		font-size: 0.34rem;
		text-align: center;
	}
</style>