<template>
	<view class="detail">
		<view class="title">
			文章的标题文章的标题文章的标题文章的标题文章的标题文章的标题文章的标题
		</view>
		<view class="info">
			<view class="author">编辑：张三</view>
			<view class="time">2022-12-12 15:30:00</view>		
		</view>
		<view class="content">
			内容部分
		</view>
		<view class="description">
			声明：本站的内容均采集与腾讯新闻，如果侵权请联系管理（513894357@qq.com）进行整改删除，本站进行了内容采集不代表本站及作者观点，若有侵犯请及时联系管理员，谢谢您的支持。
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
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
