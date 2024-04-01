<template>
	<view class="page-container">
		<view id="top-decoration" :animation="animationFillLogo">
			<image src="/static/welcome/fillInLogo.png" mode="aspectFit" />
		</view>
		<view id="title-content" :animation="animationTitle">
			<view class="company-text">
				<text>{{companyName}}</text>
			</view>
			<view class="title-text">
				<text>{{title}}</text>
			</view>
		</view>
		<view id="describe-content" :animation="animationBottom">
			<view class="start-button">
				<button :disabled="disabledFillInClick" @click="onStartFillIn()">开始填写</button>
			</view>
			<image :src="companyLogoPath" mode="widthFix" class="logo" />
		</view>
		<view class="bottom-decoration" :animation="animationBottom"></view>
		<view id="fill-in-container" :style="{ height: 'calc(100vh - ' + fillContainerHeight + 'px)' }"
			:animation="animationFillIn">
			<view class="fill-in-panel">
				<uni-forms>
					<uni-group title="搬送物">
						<uni-forms-item name="transportMachine">
							<uni-data-checkbox v-model="fillInData.transportMachine"
								:localdata="formTransportMachine" />
						</uni-forms-item>
					</uni-group>
					<uni-group title="托盘尺寸及重量">
						<uni-group title="最小">
							<uni-forms-item label-width="140" label="宽">
								<uni-easyinput v-model="fillInData.trayData.min.width" placeholder="请输入最小宽" />
								<text class="unit">mm</text>
							</uni-forms-item>
							<uni-forms-item label-width="140" label="长">
								<uni-easyinput v-model="fillInData.trayData.min.length" placeholder="请输入最小长" />
								<text class="unit">mm</text>
							</uni-forms-item>
							<uni-forms-item label-width="140" label="高">
								<uni-easyinput v-model="fillInData.trayData.min.height" placeholder="请输入最小高" />
								<text class="unit">mm</text>
							</uni-forms-item>
							<uni-forms-item label-width="140" label="重量">
								<uni-easyinput v-model="fillInData.trayData.min.weight" placeholder="请输入最小重量" />
								<text class="unit">kg</text>
							</uni-forms-item>
						</uni-group>
						<uni-group title="最大">
							<uni-forms-item label-width="140" label="宽">
								<uni-easyinput v-model="fillInData.trayData.max.width" placeholder="请输入最大宽" />
								<text class="unit">mm</text>
							</uni-forms-item>
							<uni-forms-item label-width="140" label="长">
								<uni-easyinput v-model="fillInData.trayData.max.length" placeholder="请输入最大长" />
								<text class="unit">mm</text>
							</uni-forms-item>
							<uni-forms-item label-width="140" label="高">
								<uni-easyinput v-model="fillInData.trayData.max.height" placeholder="请输入最大高" />
								<text class="unit">mm</text>
							</uni-forms-item>
							<uni-forms-item label-width="140" label="重量">
								<uni-easyinput v-model="fillInData.trayData.max.weight" placeholder="请输入最大重量" />
								<text class="unit">kg</text>
							</uni-forms-item>
						</uni-group>
					</uni-group>
					<uni-group title="使用装卸机器">
						<uni-forms-item name="unloadingMachine">
							<uni-data-checkbox v-model="fillInData.unloadingMachine"
								:localdata="formUnloadingMachine" />
						</uni-forms-item>
					</uni-group>
					<uni-group title="最大搬送能力">
						<uni-forms-item label-width="140" label="1F~2F">
							<uni-easyinput v-model="fillInData.maxTransportAbility.f1t2" placeholder="请输入1F~2F最大搬送能力" />
							<text class="unit">托盘/时</text>
						</uni-forms-item>
						<uni-forms-item label-width="140" label="1F~3F">
							<uni-easyinput v-model="fillInData.maxTransportAbility.f1t3" placeholder="请输入1F~3F最大搬送能力" />
							<text class="unit">托盘/时</text>
						</uni-forms-item>
						<uni-forms-item label-width="140" label="1F~4F">
							<uni-easyinput v-model="fillInData.maxTransportAbility.f1t4" placeholder="请输入1F~4F最大搬送能力" />
							<text class="unit">托盘/时</text>
						</uni-forms-item>
					</uni-group>
					<uni-group title="搬运物最大尺寸">
						<uni-forms-item label-width="140" label="宽">
							<uni-easyinput v-model="fillInData.transportObjectSize.width" placeholder="请输入宽" />
							<text class="unit">mm</text>
						</uni-forms-item>
						<uni-forms-item label-width="140" label="长">
							<uni-easyinput v-model="fillInData.transportObjectSize.length" placeholder="请输入长" />
							<text class="unit">mm</text>
						</uni-forms-item>
						<uni-forms-item label-width="140" label="高">
							<uni-easyinput v-model="fillInData.transportObjectSize.height" placeholder="请输入高" />
							<text class="unit">mm</text>
						</uni-forms-item>
					</uni-group>
					<uni-group title="搬送方向类型">
						<uni-forms-item>
							<uni-data-checkbox v-model="fillInData.transportDirection"
								:localdata="formTransportDirection" />
						</uni-forms-item>
						<image @click="previewImage(transportDirectionImgPath)" :src="transportDirectionImgPath"
							mode="widthFix" class="transport-direction-img" />
					</uni-group>
					<uni-group title="楼层高度">
						<uni-forms-item label-width="140" label="1F">
							<uni-easyinput v-model="fillInData.floorHeight.f1" placeholder="请输入1F高度" />
							<text class="unit">mm</text>
						</uni-forms-item>
						<uni-forms-item label-width="140" label="2F">
							<uni-easyinput v-model="fillInData.floorHeight.f2" placeholder="请输入2F高度" />
							<text class="unit">mm</text>
						</uni-forms-item>
						<uni-forms-item label-width="140" label="3F">
							<uni-easyinput v-model="fillInData.floorHeight.f3" placeholder="请输入3F高度" />
							<text class="unit">mm</text>
						</uni-forms-item>
						<uni-forms-item label-width="140" label="4F">
							<uni-easyinput v-model="fillInData.floorHeight.f4" placeholder="请输入4F高度" />
							<text class="unit">mm</text>
						</uni-forms-item>
						<uni-forms-item label-width="140" label="5F">
							<uni-easyinput v-model="fillInData.floorHeight.f5" placeholder="请输入5F高度" />
							<text class="unit">mm</text>
						</uni-forms-item>
						<uni-forms-item label-width="140" label="6F">
							<uni-easyinput v-model="fillInData.floorHeight.f6" placeholder="请输入6F高度" />
							<text class="unit">mm</text>
						</uni-forms-item>
						<uni-forms-item label-width="140" label="7F">
							<uni-easyinput v-model="fillInData.floorHeight.f7" placeholder="请输入7F高度" />
							<text class="unit">mm</text>
						</uni-forms-item>
						<uni-forms-item label-width="140" label="8F">
							<uni-easyinput v-model="fillInData.floorHeight.f8" placeholder="请输入8F高度" />
							<text class="unit">mm</text>
						</uni-forms-item>
					</uni-group>
					<uni-group title="环境">
						<uni-forms-item name="environment">
							<uni-data-checkbox v-model="fillInData.environment.type" :localdata="formEnvironment" />
						</uni-forms-item>
						<uni-forms-item label-width="140" label="防火卷帘门" name="environment.fastSafeDoor">
							<uni-data-checkbox v-model="fillInData.environment.fastSafeDoor" :localdata="formHaveNot" />
						</uni-forms-item>
						<uni-forms-item label-width="140" label="快速安全门" name="environment.firewallDoor">
							<uni-data-checkbox v-model="fillInData.environment.firewallDoor" :localdata="formHaveNot" />
						</uni-forms-item>
					</uni-group>
					<uni-group title="建筑尺寸">
						<uni-forms-item label-width="140" label="井道尺寸">
							<uni-easyinput v-model="fillInData.buildingSize.shafts" placeholder="请输入井道尺寸" />
						</uni-forms-item>
						<uni-forms-item label-width="140" label="楼板洞口尺寸">
							<uni-easyinput v-model="fillInData.buildingSize.floorHoles" placeholder="请输入最大宽楼板洞口尺寸" />
						</uni-forms-item>
					</uni-group>
					<uni-group title="是否系统对接">
						<uni-forms-item name="systemJoint">
							<uni-data-checkbox v-model="fillInData.systemJoint" :localdata="formSystemJoint" />
						</uni-forms-item>
					</uni-group>
					<uni-group title="布局图是否提供">
						<uni-forms-item name="layoutprovide">
							<uni-data-checkbox v-model="fillInData.layoutprovide" :localdata="formLayoutprovide" />
						</uni-forms-item>
					</uni-group>
					<uni-group title="客户名称">
						<uni-forms-item name="customerName">
							<uni-easyinput v-model="fillInData.customerName" placeholder="请输入您的名称" />
						</uni-forms-item>
					</uni-group>
				</uni-forms>
				<view class="form-submit-button">
					<button>提交</button>
				</view>
				<!-- <picker-view v-if="transportDirectionPickerVisible">
					<picker-view-column>

					</picker-view-column>
				</picker-view> -->
			</view>
		</view>
	</view>
</template>

<script>
	var animationTitle = uni.createAnimation({
		duration: 700,
		timingFunction: "ease-out"
	})
	var animationBottom = uni.createAnimation({
		duration: 700,
		timingFunction: "ease-out"
	})
	var animationFillLogo = uni.createAnimation({
		duration: 700,
		timingFunction: "ease-out"
	})
	var animationFillIn = uni.createAnimation({
		duration: 200,
		timingFunction: "ease-out"
	})
	export default {
		data() {
			return {
				animationTitle: {},
				animationBottom: {},
				animationFillLogo: {},
				animationFillIn: {},
				companyLogoPath: '/static/welcome/company-logo.png',
				transportDirectionImgPath: '/static/welcome/transportDirection.png',
				title: '垂直物流输送系统前期规划调查表',
				companyName: '深圳欧格瑞科技',
				disabledFillInClick: false,
				systemInfo: {},
				// transportDirectionPickerVisible: false,
				fillContainerHeight: 0,
				formTransportMachine: [{
						text: '托盘',
						value: '托盘'
					},
					{
						text: '笼车',
						value: '笼车'
					}
				],
				formUnloadingMachine: [{
						text: '叉车',
						value: '叉车'
					},
					{
						text: '人工叉车',
						value: '人工叉车'
					},
					{
						text: '手推车',
						value: '手推车'
					}
				],
				formSystemJoint: [{
						text: 'WMS',
						value: 'WMS'
					},
					{
						text: 'ERP',
						value: 'ERP'
					},
					{
						text: 'AGV',
						value: 'AGV'
					},
				],
				formLayoutprovide: [{
						text: '提供',
						value: '提供'
					},
					{
						text: '不提供',
						value: '不提供'
					},
				],
				formTransportDirection: [{
						text: 'Z型',
						value: 'Z型'
					},
					{
						text: 'C型',
						value: 'C型'
					},
					{
						text: 'CC型',
						value: 'CC型'
					},
					{
						text: 'CZ型',
						value: 'CZ型'
					},
					{
						text: 'ZC型',
						value: 'ZC型'
					},
					{
						text: 'F型',
						value: 'F型'
					},
					{
						text: 'E型',
						value: 'E型'
					},
					{
						text: 'ZE型',
						value: 'ZE型'
					},
					{
						text: 'FE型',
						value: 'FE型'
					},
					{
						text: 'EE型',
						value: 'EE型'
					},
					{
						text: 'LF型',
						value: 'LF型'
					},
					{
						text: 'LL型',
						value: 'LL型'
					},
				],
				formEnvironment: [{
						text: '新建',
						value: '新建'
					},
					{
						text: '已建',
						value: '已建'
					},
					{
						text: '室内',
						value: '室内'
					},
					{
						text: '室外',
						value: '室外'
					},
					{
						text: '冷藏',
						value: '冷藏'
					},
					{
						text: '常温',
						value: '常温'
					},
				],
				formHaveNot: [{
						text: '有',
						value: '有'
					},
					{
						text: '无',
						value: '无'
					},
				],
				fillInData: {
					customerName: null,
					transportMachine: null,
					unloadingMachine: null,
					maxTransportAbility: {
						f1t2: null,
						f1t3: null,
						f1t4: null
					},
					transportObjectSize: {
						width: null,
						height: null,
						length: null
					},
					transportDirection: null,
					floorHeight: {
						f1: null,
						f2: null,
						f3: null,
						f4: null,
						f5: null,
						f6: null,
						f7: null,
						f8: null,
					},
					environment: {
						type: null,
						fastSafeDoor: false,
						firewallDoor: false
					},
					buildingSize: {
						shafts: null,
						floorHoles: null
					},
					systemJoint: null,
					layoutprovide: false,
					trayData: {
						other: null,
						min: {
							length: null,
							width: null,
							height: null,
							weight: null
						},
						max: {
							length: null,
							width: null,
							height: null,
							weight: null
						}
					}
				}
			}
		},
		methods: {
			previewImage(url) {
				// uni.previewImage({urls: [url]})
			},
			onStartFillIn(e) {
				// uni.redirectTo({
				// 	url: '/pages/fillIn/fillIn'
				// })
				this.disabledFillInClick = true;

				let bottomElement = uni.createSelectorQuery().select("#describe-content");
				bottomElement.boundingClientRect(rect => {
					animationBottom.translateY(rect.height).step();
					this.animationBottom = animationBottom.export();
				}).exec();

				let titleElement = uni.createSelectorQuery().select("#title-content");
				titleElement.boundingClientRect(rect => {
					animationTitle.translateY(-rect.top).step()
					this.animationTitle = animationTitle.export();
					this.fillContainerHeight = rect.height;

					setTimeout(() => {
						animationFillIn.opacity(1).step();
						this.animationFillIn = animationFillIn.export();
					}, 700)
				}).exec();

				let fillLogoElement = uni.createSelectorQuery().select("#top-decoration");
				fillLogoElement.boundingClientRect(rect => {
					animationFillLogo.opacity(0).step();
					this.animationFillLogo = animationFillLogo.export();
				}).exec();

			}
		},
		onload() {
			const systemInfo = uni.getSystemInfoSync();
			this.systemInfo = systemInfo;
		}
	}
</script>

<style lang="scss" scoped>
	#fill-in-container {
		position: absolute;
		overflow: hidden;
		bottom: 0;
		left: 0;
		right: 0;
		z-index: 100;
		opacity: 0;
		height: 0;

		.fill-in-panel {
			width: 100%;
			height: 100%;
			overflow: scroll;
		}
	}

	.page-container {
		position: relative;
		overflow: hidden;
		width: 100vw;
		height: 100vh;
		display: flex;
		flex-direction: column;
		justify-content: flex-end;
		background-color: white;
	}

	#top-decoration {
		flex: 1;
		display: flex;
		justify-content: center;
		align-items: center;
		overflow: hidden;

		image {
			width: 100%;
			height: 100%;
			display: block;
		}
	}

	.bottom-decoration {
		position: absolute;
		height: 240rpx;
		background-color: orange;
		opacity: .2;
		border-radius: 100% / 100%;
		right: 0rpx;
		left: 0rpx;
		bottom: 0;
		transform: translateY(56%);
	}

	#title-content {
		z-index: 99;
		width: 100%;
		padding: 20rpx 0;

		.company-text {
			font-size: 50rpx;
			text-align: center;
		}

		.title-text {
			font-size: 40rpx;
			text-align: center;
		}
	}

	#describe-content {
		z-index: 101;
		width: 100%;
		padding: 20rpx 0;

		.logo {
			width: 50%;
			display: block;
			margin: 0 auto;
			margin-bottom: 20rpx;
		}

		.start-button {
			padding: 0 120rpx;
			margin-bottom: 120rpx;
			margin-top: 80rpx;

			button {
				font-size: 30rpx;
				border-radius: 100rpx;
				box-shadow: 2px 2px 10px rgba(0, 0, 0, .2);
			}

			button::after {
				border: none;
			}
		}
	}

	.unit {}

	::v-deep .uni-forms-item__content {
		display: flex;
		align-items: center;

		.unit {
			margin: 0 20rpx;
		}
	}

	::v-deep .uni-forms-item__label {
		text {
			width: 100%;
			text-align: right;
		}
	}

	::v-deep .uni-group__content {}

	.transport-direction-img {
		width: 100%;
	}
	
	.form-submit-button {
		padding: 20rpx;
		
		button {
			font-size: 30rpx;
			border-radius: 100rpx;
			box-shadow: 2px 2px 10px rgba(0, 0, 0, .2);
		}
	}
</style>