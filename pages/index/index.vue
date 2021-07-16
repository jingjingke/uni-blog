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
		<view class="sort-wrap modular">
			<view class="sort-entry">
				<view class="iconfont icon-note" />
				<text>前端笔记</text>
			</view>
			<view class="sort-entry">
				<view class="iconfont icon-share" />
				<text>转载分享</text>
			</view>
			<view class="sort-entry">
				<view class="iconfont icon-life" />
				<text>生活锁事</text>
			</view>
		</view>
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

	.sort-wrap {
		display: flex;
		justify-content: space-between;
	}
	.sort-entry {
		background: #fff;
		border-radius: 3px;
		width: 33%;
		text-align: center;
		padding: 24px 0 12px;
	}
	.sort-entry:nth-child(2) {
		margin: 0 12px;
	}
	.sort-entry .iconfont{
		font-size: 24px;
		padding-bottom: 12px;
		color: #0aba07;
	}
</style>
