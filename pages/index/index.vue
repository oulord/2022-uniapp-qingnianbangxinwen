<template>
	<view class="home">
		<scroll-view scroll-x class="navscroll">
			<view class="item" :class="index == navIndex ? 'active' : ' '" v-for="(item,index) in navArr"
				@click="clickNav(index)" :key="item.id">
				{{item.classname}}
			</view>
		</scroll-view>
		<view class="content">
			<view class="row" v-for="item in newsArr" :key="item.id">
				<newsbox :item="item" @click.native="goDetail"></newsbox>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: 0,
				navArr: [],
				newsArr:[]
			}
		},
		onLoad() {
			this.getNavDat();
			this.getNewsData();
		},
		methods: {
			// 点击导航切换
			clickNav(index) {
				this.navIndex = index
			},

			// 跳转到详情页
			goDetail() {
				uni.navigateTo({
					url: "/pages/detail/detail"
				})
			},

			// 获取导航列表数据
			getNavDat() {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/navlist.php",
					success: res => {
						console.log(res);
						this.navArr = res.data
					}
				})
			},

			// 获取新闻列表数据
			getNewsData() {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/newslist.php",
					data:{
						num:5,
						cid:50,
					}
					success: res => {
						console.log(res);
						this.newsArr = res.data
					}
				})
			},
		}
	}
</script>

<style lang="scss" scoped>
	.navscroll {
		height: 100rpx;
		background-color: #f7f8fa;
		white-space: nowrap;
		position: fixed;
		top: var(--window-top);
		z-index: 10;

		/deep/::-webkit-scrollbar {
			width: 4px !important;
			height: 1px !important;
			overflow: auto !important;
			background: transparent !important;
			-webkit-appearance: auto !important;
			display: block;
		}

		.item {
			font-size: 40rpx;
			display: inline-block;
			line-height: 100rpx;
			padding: 0 30rpx;
			color: #333;

			&.active {
				color: #31C27C;
			}
		}
	}

	.content {
		padding: 30rpx;
		padding-top: 130rpx;

		.row {
			border-bottom: 1px dotted #efefef;
			padding: 20rpx 0;
		}
	}
</style>
