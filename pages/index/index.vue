<template>
	<view>
		<swiper class="swiper-wrap modular" :interval='10000' :autoplay='true'>
			<swiper-item v-for='(item,index) in swiperList' :key='index'>
				<view class="swiper-item">
					<image class="swiper-image modular" :src="item.litpic"></image>
					<text class="swiper-text">{{item.title}}</text>
				</view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				swiperList: []
			}
		},
		onLoad() {
			this.querySwiperList();
		},
		methods: {
			querySwiperList() {
				uni.request({
					url: 'http://jingjingke.com/api/list.php',
					data: {
						typeid: 2,
						flag: true
					},
					success: (res) => {
						this.swiperList = (res.data || []).map(item => {
							return {
								litpic: 'http://www.jingjingke.com' + item.litpic,
								title: item.title
							}
						})
					}
				});
			}
		}
	}
</script>
<style>
	.swiper-wrap {
		height: 300px;
		background: #fff;
	}

	.swiper-image {
		display: block;
		width: auto;
	}

	.swiper-text {
		display: block;
		height: 3em;
		line-height: 3em;
		margin: 0 20px;
		text-align: center;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}
</style>
