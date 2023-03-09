<template>
	<view class="home">
		<!-- 循环导航栏 -->
		<scroll-view scroll-x class="navscroll">
			<view class="item" :class="index == navIndex ? 'active' : ' '" v-for="(item,index) in navArr"
				@click="clickNav(index,item.id)" :key="item.id">
				{{item.classname}}
			</view>
		</scroll-view>
		
		<!-- 循环新闻 -->
		<view class="content">
			<view class="row" v-for="item in newsArr" :key="item.id">
				<newsbox :item="item" @click.native="goDetail"></newsbox>
			</view>
		</view>

		<!-- 当没有新闻的页面 -->
		<view class="nodata" v-if=!newsArr.length>
			<!-- node=widthFix:宽度不变,高度自动变化,保持原图宽高比不变 -->
			<image src="../../static/images/nodata.png" mode="widthFix"></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: 0,
				navArr: [], //导航栏数据
				newsArr: [] ,//新闻数据
				currentPage:1,  //定义第一页新闻
			}
		},
		// 第一次进入页面执行
		onLoad() {
			this.getNavDat();
			this.getNewsData();
		},
		onReachBottom(){
			// 对触底后的 currentPage 进行加页,并且重新请求 currentPage为2 的新闻数据
			// console.log("到底部了");
			this.currentPage++;
			this.getNewsData();
		},
		methods: {
			// 点击导航切换
			clickNav(index, id) {
				this.navIndex = index;
				// 切换导航栏是让page重新为1，并且不能放在 this.getnewsData() 之下，因为要在请求新闻数据之前，让页面为1
				this.currentPage=1;
				// 将新闻数据重新清空
				this.newsArr=[];
				// console.log(id);  id为 50、51、52……
				this.getNewsData(id);				
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
						// 将获取的导航列表的数据放入 navArr 数组中
						this.navArr = res.data
					}
				})
			},

			// 获取新闻列表数据
			getNewsData(id = 50) {
				uni.request({
					url: "https://ku.qingnian8.com/dataApi/news/newslist.php",
					data: {
						cid: id,
						// 获取第一页新闻数据
						page:this.currentPage,
					},
					success: res => {
						console.log(res);
						// 将获取的新闻列表数据 放入 newsArr 数组中进行拼接
						this.newsArr = [...this.newsArr,...res.data]
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

	.nodata {
		display: flex;
		justify-content: center;

		image {
			width: 360rpx;

		}
	}
</style>
