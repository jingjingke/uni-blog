<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
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
	/*每个页面公共css */
	/* 字体图标 */
	@import url("./static/iconfont/iconfont.css");
	body {
		background: #f2f2f2;
	}
	/* 页面中的一个模块间距 */
	.modular {
		margin: 12px 12px 0;
		border-radius: 5px;
	}
</style>
