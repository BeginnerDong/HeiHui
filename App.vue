
<script>
	import token from "./common/token.js"
	export default {
		onLaunch: function() {	
			if(!uni.getStorageSync('user_token')){
				this.$Router.redirectTo({route:{path:'/pages/login-register/login-register'}})
			}
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/* 富文本样式 */
	@import "/assets/style/quill.css";
	
	
	/* 公共样式 */
	@import "/common/css/common.css";
	/* 公共样式 */
	@import "/common/css/main.css";
	/* html{height: auto;} */
	/* body{background-color: #fff!important;} */
</style>
