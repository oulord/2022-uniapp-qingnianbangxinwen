<template>
	<view class="detail">
		<view class="title">
			{{detail.title}}
		</view>
		<view class="info">
			<view class="author">编辑：{{detail.author}}</view>
			<view class="time">时间：{{detail.posttime}}</view>		
		</view>
		<view class="content">
			<rich-text :nodes="detail.content">
				<!-- 用v-html也行，但不建议这么使用，有安全问题 -->
			<!-- <view v-html="detail.content"></view> -->
			</rich-text>
		</view>
		<view class="description">
			声明：本站的内容均采集与腾讯新闻，如果侵权请联系管理（513894357@qq.com）进行整改删除，本站进行了内容采集不代表本站及作者观点，若有侵犯请及时联系管理员，谢谢您的支持。
		</view>
	</view>
</template>

<script>
	import {parseTime} from "../../utils/tool.js"
	export default {
		data() {
			return {
				options:null,
				detail:{},
			};
		},
		// 不是页面生命周期,是接收 index.vue 中的 uni.navigateTo 传过来的数据 cid 与 id
		onLoad(e){
			// console.log(e);
			this.options = e;
			this.getDetail()
		},
		methods:{
			getDetail(){
				uni.request({
					url:"https://ku.qingnian8.com/dataApi/news/detail.php",
					data:this.options,
					success:res => {
						console.log(res);
						// 引入utils模块中的转换时间戳的方法
						res.data.posttime = parseTime(res.data.posttime)
						// 图片在微信小程序中还是没有改变,使用一个正则表达式
						res.data.content = res.data.content.replace(/<img/gi,'<img style = "max-width:100"')
						// 将请求的数据赋值给detail
						this.detail = res.data
						
						this.saveHistory()
						
						// 动态设置当前页面的导航标题
						uni.setNavigationBarTitle({
							title: this.detail.title
						});
					}
				})
			},
			// 数据缓存
			saveHistory(){
				let historyArr = uni.getStorageSync("historyArr") || []
				// 定义需要的值
				let item = {
					id:this.detail.id,
					classid:this.detail.classid,
					picurl:this.detail.picurl,
					title:this.detail.title,					
					looktime:parseTime(Date.now())
				}
				
				// 获取缓存中重复数据的最新index,使用 findIndex 方法,i 为 当前元素 => historyArr
				let index = historyArr.findIndex((i) => {
					return i.id == this.detail.id
				})
				
				// 对数据去重
				if(index >= 0){
					historyArr.splice(index,1)
				}
				
				historyArr.unshift(item)
				uni.setStorageSync("historyArr",historyArr)
			}
		}
	}
</script>

<style lang="scss">
	.detail{
		padding: 30rpx;
		.title{
			font-size: 46rpx;
			color: #333;
		}
		.info{
			background: #f6f6f6;
			padding: 20rpx;
			font-size: 25rpx;
			color: #666;
			display: flex;
			justify-content: space-between;
			margin: 40rpx 0;
		}
		.content{
			padding-bottom: 50rpx;
			/deep/ img{
				width: 100%;
			}
		}
		.description{
			background: #fef0f0;
			font-size: 26rpx;
			padding: 20rpx;
			color:#f89898;
			line-height: 1.8em;
		}
	}

</style>
