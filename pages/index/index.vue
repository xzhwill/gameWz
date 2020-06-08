<template>
	<view class="content">
		<view class="game-time">{{hours}}:{{mins}}:{{secs}}</view>
		<view class="content-bg">
			<view class="remove-box">
				<view class="game-ul">
					<view class="game-li" @click="checkClick(item.src,index)" :class="[item.onFlag?'on':'',item.hideFlag?' hide':'']"
					 :style="'left:'+item.left+'rpx;top:'+item.top+'rpx'" v-for="(item,index) in positionList" :key="index">
						<image :src="item.src" mode=""></image>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				positionList: [],
				hasCheck: false,
				checkSrc: "",
				checkIndex: -1,
				hours: "00",
				mins: "00",
				secs: "00",
				setId: "",
				setFlag: true,
				setNum: 0
			}
		},
		onLoad() {
			this.init();
		},
		methods: {
			// 页面初始
			init() {
				let randomNumList = [];
				while (randomNumList.length < 17) {
					let num = Math.floor(Math.random() * 75);
					if (randomNumList.indexOf(num) == -1) {
						randomNumList.push(num);
					}
				}
				let twiRandomNumList = randomNumList.concat(randomNumList);
				let newTwiRandomNumList = [];
				while (newTwiRandomNumList.length != 34) {
					let twiRandomNumListIndex = Math.floor(Math.random() * (twiRandomNumList.length - 1))
					let randomNum = twiRandomNumList[twiRandomNumListIndex];
					newTwiRandomNumList.push(randomNum);
					twiRandomNumList.splice(twiRandomNumListIndex, 1);
				}
				for (let i = 0; i < newTwiRandomNumList.length; i++) {
					newTwiRandomNumList[i] += 1;
				}
				let positionList = [];
				for (let i = 1; i <= 34; i++) {
					let newTwiRandomNumListIndex = Math.floor(Math.random() * (newTwiRandomNumList.length - 1))
					let everyNum = newTwiRandomNumList[newTwiRandomNumListIndex];
					newTwiRandomNumList.splice(newTwiRandomNumListIndex, 1);
					let everySrc = "../../static/images/small" + everyNum + ".png";
					if (i >= 1 && i <= 4) {
						positionList.push({
							src: everySrc,
							left: 24 + (i - 1) * 70,
							top: 18,
							onFlag: false,
							hideFlag: false
						})
					} else if (i >= 5 && i <= 8) {
						positionList.push({
							src: everySrc,
							left: 332 + (i - 5) * 70,
							top: 18,
							onFlag: false,
							hideFlag: false
						})
					} else if (i >= 9 && i <= 10) {
						positionList.push({
							src: everySrc,
							left: 102 + (i - 9) * 309,
							top: 101,
							onFlag: false,
							hideFlag: false
						})
					} else if (i >= 11 && i <= 14) {
						positionList.push({
							src: everySrc,
							left: 24 + (i - 11) * 70,
							top: 183,
							onFlag: false,
							hideFlag: false
						})
					} else if (i >= 15 && i <= 18) {
						positionList.push({
							src: everySrc,
							left: 332 + (i - 15) * 70,
							top: 183,
							onFlag: false,
							hideFlag: false
						})
					} else if (i >= 19 && i <= 20) {
						positionList.push({
							src: everySrc,
							left: 78 + (i - 19) * 156,
							top: 262,
							onFlag: false,
							hideFlag: false
						})
					} else if (i >= 21 && i <= 22) {
						positionList.push({
							src: everySrc,
							left: 386 + (i - 21) * 156,
							top: 262,
							onFlag: false,
							hideFlag: false
						})
					} else if (i >= 23 && i <= 24) {
						positionList.push({
							src: everySrc,
							left: 66 + (i - 23) * 168,
							top: 338,
							onFlag: false,
							hideFlag: false
						})
					} else if (i >= 25 && i <= 26) {
						positionList.push({
							src: everySrc,
							left: 373 + (i - 25) * 169,
							top: 338,
							onFlag: false,
							hideFlag: false
						})
					} else if (i >= 27 && i <= 30) {
						positionList.push({
							src: everySrc,
							left: 29 + (i - 27) * 70,
							top: 422,
							onFlag: false,
							hideFlag: false
						})
					} else if (i >= 31 && i <= 34) {
						positionList.push({
							src: everySrc,
							left: 337 + (i - 31) * 70,
							top: 422,
							onFlag: false,
							hideFlag: false
						})
					}
				}
				this.positionList = positionList;
			},
			checkClick(src, index) {
				let _this = this;
				if (this.setFlag) {
					this.setFlag = false;
					this.setId = setInterval(function() {
						_this.setNum++;
						let setHours = Math.floor(_this.setNum / 3600);
						let setMins = Math.floor(_this.setNum / 60);
						let setSecs = _this.setNum % 60;
						_this.hours = setHours < 10 ? "0" + setHours : setHours;
						_this.mins = setMins < 10 ? "0" + setMins : setMins;
						_this.secs = setSecs < 10 ? "0" + setSecs : setSecs;
					}, 1000)
				}
				if (this.positionList[index].onFlag) { //当前点击的这个图如果是被选中的状态的则取消选中
					this.clearpositionList();
				} else {
					if (this.checkSrc == "") { //如果当前没有被选中的图
						this.positionList[index].onFlag = true;
						this.checkSrc = this.positionList[index].src; //设置选中的图路径
						this.checkIndex = index;
					} else { //如果当前有被选中的图
						if (src == this.checkSrc) { //选择的图路径一样
							if (index == this.checkIndex) { //选的是之前选中的图则提示相同的头像
								this.clearpositionList();
							} else { //这是对的消除这2个图
								this.positionList[index].hideFlag = true;
								this.positionList[this.checkIndex].hideFlag = true;
								this.checkSrc = "";
								this.checkIndex = -1;
								let hasallhideFlag = false;
								for (let i = 0; i < this.positionList.length; i++) {
									if (this.positionList[i].hideFlag == false) {
										hasallhideFlag = true;
										break;
									}
								}
								if (!hasallhideFlag) { //如果hasallhideFlag是false，则hideFlag都是true，也就是都消除了
									uni.showToast({
										title: "恭喜你，都消除了哦，棒棒的~！",
										icon: 'none'
									})
									clearInterval(this.setId);
								}
							}
						} else { //选择的图路径不一样
							this.clearpositionList();
						}
					}
				}
			},
			clearpositionList() {
				// 相同头像才能消除哦~!
				uni.showToast({
					title: "相同头像才能消除哦~!",
					icon: 'none'
				})
				this.checkSrc = "";
				this.checkIndex = -1;
				for (let i = 0; i < this.positionList.length; i++) {
					this.positionList[i].onFlag = false;
				}
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.content-bg {
		position: fixed;
		/* z-index: 9999; */
		left: 50%;
		top: 60%;
		transform: translate(-50%, -50%);
		padding-top: 270rpx;
		width: 100%;
		height: 952rpx;
		background: url("../../static/images/remove_bg.png") no-repeat 50% 0;
		background-size: 100% 952rpx;
	}

	.game-time {
		font-size: 40rpx;
		height: 200rpx;
		line-height: 200rpx;
	}

	.game-ul {
		position: relative;
		width: 650rpx;
		height: 520rpx;
		margin: 0 auto;
		overflow: hidden;
	}

	.game-li {
		width: 63rpx;
		height: 63rpx;
		position: absolute;
		border: 2rpx solid #8298ed;
		border-radius: 0.1rem;
		-webkit-border-radius: 10rpx;
		-moz-border-radius: 10rpx;
		-ms-border-radius: 10rpx;
		-o-border-radius: 10rpx;
		transition: all 0.1s ease-out;
	}

	.game-ul .game-li.on {
		border: 2rpx solid red;
		transform: scale(1.1);
	}

	.game-ul .game-li.hide {
		transform: scale(0);
	}

	.game-li image {
		width: 100%;
		height: 100%;
		display: block;
	}
</style>
