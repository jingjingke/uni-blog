<template>
	<scroll-view class="scroll-wrap" :scroll-y='true' @scrolltolower='queryList'>
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

		<view v-for='item of list' :key='item.aid' class="scroll-list">
			<image :src="item.litpic" :class="['list-image',{
				hide:item.litpic===''
			}]"></image>
			<view class="list-content">
				<text class="list-title">{{item.title}}</text>
				<text class="list-desc">{{item.description}}</text>
				<view class="list-time-wrap">
					<view class="iconfont icon-calendar" />
					<text class="list-time">{{item.senddate}}</text>
				</view>
			</view>
		</view>
		<view class="list-loading-text">
			<text>{{scrollStatus?'正在加载中...':'别向下拖了，人家也是有底线的呐'}}</text>
		</view>
	</scroll-view>
</template>

<script>
	export default {
		data() {
			return {
				size: 5,
				listStatus: false,
				list: [],
				scrollStatus: true,
				swiperList: []
			}
		},
		onLoad() {
			this.querySwiperList();
			this.queryList();
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
								...item,
								litpic: item.litpic ? 'http://www.jingjingke.com' + item.litpic : ''
							}
						})
					}
				});
			},
			queryList() {
				if(!this.scrollStatus){
					return;
				}
				uni.request({
					url: 'http://jingjingke.com/api/list.php',
					data: {
						typeid: 1,
						limit: this.list.length + ',' + this.size
					},
					success: (res) => {
						let array = res.data || [];
						this.list.push(...array.map(item => {
							return {
								...item,
								litpic: item.litpic ? 'http://www.jingjingke.com' + item.litpic :
									''
							}
						}))
						if (array.length < this.size || this.list.length >= 20) {
							this.scrollStatus = false;
						}
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

	.sort-entry .iconfont {
		font-size: 24px;
		padding-bottom: 12px;
		color: #0aba07;
	}

	.scroll-wrap {
		height: 100%
	}

	.scroll-list {
		display: flex;
		background: #fff;
		border-radius: 3px;
		padding: 12px;
		margin: 12px;
	}

	.list-image {
		display: block;
		width: 108px;
		height: 80px;
		margin-right: 12px;
	}

	.list-content {
		flex: 1;
		overflow: hidden;
	}

	.list-title {
		display: block;
		height: 1.5em;
		line-height: 1.5em;
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
	}

	.list-desc {
		display: block;
		margin-top: 3px;
		font-size: 12px;
		line-height: 1.5em;
		height: 3em;
		overflow: hidden;
	}

	.list-time-wrap {
		display: flex;
		color: #999;
		align-items: center;
		font-size: 12px;
		margin-top: 5px;
	}

	.list-time {
		padding-left: 3px;
	}

	.list-time-wrap .iconfont {
		font-size: 12px;
	}

	.hide {
		display: none;
	}

	.list-loading-text {
		text-align: center;
		color: #999;
		font-size: 12px;
		line-height: 3em;
	}
</style>
