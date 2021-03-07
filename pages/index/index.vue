<template>
	<view class="content">
		<view class="big-box">
			<scroll-view scroll-x="true" class="scroll-Y" @scroll="scroll">
				<view class="scroll-item" v-for="(item,index) in list" :key="index">
					<view class="item-box" v-for="(item2,index2) in item" :key="index2">
						<view class="item-top"></view>
						<view>{{item2.name}}</view>
					</view>
				</view>
			</scroll-view>
			<view class="row-box">
				<view class="row-item" :style="{width:rowWidth+'%',transform: 'translateX('+move+')'}"></view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				list: [],
				rowWidth: 0,
				screenWidth: 0,
				singleWidth: 0,
				totalWidth: 0,
				move: 0
			}
		},
		onLoad() {
			for (let i = 0; i < 25; i++) {
				this.list.push({
					name: `品类${i+1}`
				})
			}
			this.list = this.group(this.list, 2)
			this.calcWidth()
			uni.getSystemInfo({
				success: (res) => {
					this.screenWidth = res.windowWidth
					this.singleWidth = this.screenWidth * 0.2
					this.totalWidth = this.singleWidth * this.list.length
				}
			});
		},
		methods: {
			/* 计算小横条宽度 */
			calcWidth() {
				if (this.list.length <= 5) {
					this.rowWidth = 100
				} else {
					this.rowWidth = (5 / this.list.length) * 100
				}
			},
			/* 拆分数组 */
			group(array, subGroupLength) {
				let index = 0;
				let newArray = [];
				while (index < array.length) {
					newArray.push(array.slice(index, index += subGroupLength));
				}
				return newArray;
			},
			/* 滚动事件 */
			scroll(e) {
				const query = uni.createSelectorQuery().in(this);
				query.select('.row-box').boundingClientRect(rowData => {
					let scrollLeft = e.target.scrollLeft //移动的偏移量
					let ratio = ((scrollLeft / this.totalWidth)) //移动的比例
					this.move = rowData.width * ratio + 'px'
				}).exec();
			}
		}
	}
</script>

<style lang="scss">
	.row-item {
		width: 50%;
		height: 100%;
		background: #b57900;
		border-radius: 20rpx !important;
		transition: all 0.1s linear;
	}

	.row-box {
		width: 100rpx;
		height: 10rpx;
		border-radius: 20rpx;
		background: #999999;
		margin: 20rpx auto 0 auto;
	}

	.big-box {
		width: 100%;
		height: 250rpx;
		background: #FFF;
	}

	.item-top {
		width: 50rpx;
		height: 50rpx;
		border-radius: 50%;
		background: #ffaa7f;
		margin-bottom: 10rpx;
		margin-top: 5rpx;
	}

	.item-box {
		width: 100%;
		height: calc(100% / 2);
		display: flex;
		align-items: center;
		flex-direction: column;
	}

	.scroll-item {
		width: 20%;
		height: 100%;
		display: inline-block;
		overflow: hidden;
	}

	.scroll-Y {
		width: 100%;
		height: 100%;
		white-space: nowrap;
		background: #55aaff;
	}

	.box {
		width: 100%;
		height: 200rpx;
		overflow: hidden;
	}
</style>
