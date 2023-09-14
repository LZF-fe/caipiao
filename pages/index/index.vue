<template>
	<view>
		<view class="uni-padding-wrap">
			<uni-segmented-control :current="current" :values="items" styleType="button" @clickItem="onClickItem" />
		</view>
		<!-- 双色球 -->
		<view class="content" v-if="current === 0">
			<view class="content-text">
				<view style="display: flex; margin-bottom: 10upx;">
					<text>前区个数：</text>
					<uni-number-box v-model="beforeNumberValue" :min="6" :max="33" />
				</view>
				<view style="display: flex;">
					<text>后区个数：</text>
					<uni-number-box v-model="afterNumberValue" :min="1" :max="16" />
					<text>-->{{combination(beforeNumberValue, 6) * afterNumberValue * 2}}元</text>
				</view>
				<view class="content-text-item" v-for="(item, index) in listS">
					<uni-icons type="minus-filled" @click="cutHandle(index)"></uni-icons>
					<text>{{item}}</text>
				</view>
			</view>
		</view>
		<!-- 大乐透 -->
		<view class="content" v-if="current === 1">
			<view class="content-text">
				<view style="display: flex; margin-bottom: 10upx;">
					<text>前区个数：</text>
					<uni-number-box v-model="beforeNumberValue" :min="5" :max="35" />
				</view>
				<view style="display: flex;">
					<text>后区个数：</text>
					<uni-number-box v-model="afterNumberValue" :min="2" :max="12" />
					<text>-->{{combination(beforeNumberValue, 5) * combination(afterNumberValue, 2) * 2}}元</text>
				</view>
				<view class="content-text-item" v-for="(item, index) in listD">
					<uni-icons type="minus-filled" @click="cutHandle(index)"></uni-icons>
					<text>{{item}}</text>
				</view>
			</view>
		</view>
		<!-- 快乐8 -->
		<view class="content" v-if="current === 2">
			<view class="content-text">
				<view>
					<view>排除号码(逗号隔开)：</view>
					<uni-easyinput class="uni-mt-5" trim="all" v-model="inputValue8" placeholder="请输入内容"></uni-easyinput>
				</view>
				<view style="margin-top: 20upx;display: flex;">
					<text>玩法：</text>
					<picker mode="selector" @change="bindPickerChange" :value="gameTypeIndex" :range="gameTypeObj" range-key="label">
						<view class="uni-input">{{gameTypeObj[gameTypeIndex]["label"]}}</view>
					</picker>
				</view>
				<view style="margin-top: 20upx;display: flex;">
					<text>范围：</text>
					<uni-number-box v-model="min8" :min="1" :max="80"/>
					~
					<uni-number-box v-model="max8" :min="1" :max="80"/>
				</view>
				<view style="margin-top: 20upx;display: flex;">
					<text>个数：</text>
					<uni-number-box v-model="beforeNumberValue" :min="gameTypeObj[gameTypeIndex].value" :max="80"/>
					<text>-->{{combination(beforeNumberValue, gameTypeObj[gameTypeIndex].value) * 2}}元</text>
				</view>
				<view class="content-text-item" v-for="(item, index) in list8">
					<uni-icons type="minus-filled" @click="cutHandle(index)"></uni-icons>
					<text>{{item}}</text>
				</view>
			</view>
		</view>
		<!-- 七星彩 -->
		<view class="content" v-show="current === 3">
			<view class="content-text">
				<view class="content-text-item" v-for="(item, index) in list7">
					<uni-icons type="minus-filled" @click="cutHandle(index)"></uni-icons>
					<text>{{item}}</text>
				</view>
			</view>
		</view>
		<!-- 3d -->
		<view class="content" v-show="current === 4">
			<view class="content-text">
				<view class="content-text-item" v-for="(item, index) in list3">
					<uni-icons type="minus-filled" @click="cutHandle(index)"></uni-icons>
					<text>{{item}}</text>
				</view>
			</view>
		</view>
		<view style="display: flex; justify-content: space-between;">
			<button class="content-button" @click="addHandle">新增</button>
			<button class="content-button" style="width: 20%;" @click="resetHandle">重置</button>
			<button class="content-button" style="width: 20%;" @click="copyHandle">复制</button>
		</view>
	</view>

</template>

<script>
	let obj = {
		'0': 'listS',
		'1': 'listD',
		// '3': 'list5',
		'2': 'list8',
		'3': 'list7',
		'4': 'list3',
	}
	export default {
		data() {
			return {
				current: 0,//当前的种类下标
				items: [
					'双色球',
					'大乐透',
					'快乐8',
					'七星彩',
					'3d',
				],
				before: [],//前区
				after: [],//后区
				listS: [],//双色球列表
				listD: [],//大乐透列表
				list7: [],//7星彩列表
				// list5: [],
				list3: [],
				list8: [],//快乐8列表
				beforeNumberValue: 6,//前区个数
				afterNumberValue: 1,//后区个数
				min8: 1,//快乐8范围最小值
				max8: 80,//快乐8范围最大值
				gameTypeObj: [
					{label: '选1', value: 1},
					{label: '选2', value: 2},
					{label: '选3', value: 3},
					{label: '选4', value: 4},
					{label: '选5', value: 5},
					{label: '选6', value: 6},
					{label: '选7', value: 7},
					{label: '选8', value: 8},
					{label: '选9', value: 9},
					{label: '选10', value: 10},
				],//快乐8玩法选项
				gameTypeIndex: 0,//快乐8玩法下标
				inputValue8: '',//快乐8排除号码input框相关
			}
		},
		onLoad() {

		},
		mounted() {

		},
		methods: {
			onClickItem(e) {
				if (this.current !== e.currentIndex) {
					this.current = e.currentIndex
				}
				if (e.currentIndex === 0) {
					this.beforeNumberValue = 6
					this.afterNumberValue = 1
				}
				if (e.currentIndex === 1) {
					this.beforeNumberValue = 5
					this.afterNumberValue = 2
				}
				if (e.currentIndex === 3) {
					this.gameTypeIndex = 0
					this.beforeNumberValue = 1
				}
			},
			//根据类型新增号码
			addHandle () {
				let obj = {
					'0': 'addHandleS',
					'1': 'addHandleD',
					// '3': 'addHandle5',
					'2': 'addHandle8',
					'3': 'addHandle7',
					'4': 'addHandle3',
				}
				this[obj[this.current]]()
			},
			//根据类型重置号码
			resetHandle () {
				this[obj[this.current]] = []
			},
			//根据类型复制号码
			copyHandle () {
				let list = this[obj[this.current]]
				if (!list.length) {
					uni.showToast({
						title: '没号码复制戟把',
						image: '../../static/toast1.png'
					})
					return
				}
				let str = ``
				list.forEach(item => {
					str += `${item}\n`
				})
				uni.setClipboardData({
					data: str,
					success: () => {
						uni.showToast({
							title: `复制了${this.items[this.current]}到剪贴板`,
							image: '../../static/toast2.png'
						})
					}
				});
			},
			//根据类型删除号码
			cutHandle (index) {
				this[obj[this.current]].splice(index, 1)
			},
			//新增双色球
			addHandleS() {
				this.before = this.setResultList(this.initList(33), this.beforeNumberValue)
				this.after = this.setResultList(this.initList(16), this.afterNumberValue)
				let before = this.before.sort((a, b) => {
					return a - b;
				}).join(", ")
				let after = this.after.sort((a, b) => {
					return a - b;
				}).join(", ")
				this.listS.push(`${before}---${after}`)
			},
			//新增大乐透
			addHandleD() {
				this.before = this.setResultList(this.initList(35), this.beforeNumberValue);
				this.after = this.setResultList(this.initList(12), this.afterNumberValue);
				let before = this.before.sort((a, b) => {
					return a - b;
				}).join(", ")
				let after = this.after.sort((a, b) => {
					return a - b;
				}).join(", ")
				this.listD.push(`${before}---${after}`)
			},
			//新增七星彩
			addHandle7() {
				this.before = this.setResultList(this.initList(10, false), 6, false);
				this.after = this.setResultList(this.initList(15, false), 1, false);
				let before = this.before.join(", ")
				let after = this.after.join()
				this.list7.push(`${before}---${after}`)
			},
			//新增排列五
			addHandle5() {
				this.before = this.setResultList(this.initList(10, false), 5, false);
				let before = this.before.join(", ")
				this.list5.push(`${before}`)
			},
			//新增3d/排列三
			addHandle3() {
				this.before = this.setResultList(this.initList(10, false), 3, false);
				let before = this.before.join(", ")
				this.list3.push(`${before}`)
			},
			//新增快乐8
			addHandle8() {
				// 需要排除的数字使用正则表达式匹配英文逗号或中文逗号，并进行切割，已去重
				const items = this.inputValue8.split(/,|，/)
				const numbersArray = items.map(item => parseFloat(item))
				const uniqueNumbers = [...new Set(numbersArray)]
				
				let list = []
				for (let i = this.min8; i <= this.max8; i++) {
					if (!uniqueNumbers.includes(i)) list.push(i)
				}
				
				//不符合规则的情况：list长度为0、生成的号码个数大于list长度
				if (!list.length || this.beforeNumberValue > list.length) {
					uni.showToast({
						title: '范围或者个数不符合规则',
						image: '../../static/toast1.png'
					})
					return
				}
				
				this.before = this.setResultList(list, this.beforeNumberValue);
				let before = this.before.sort((a, b) => {
					return a - b;
				}).join(", ")
				this.list8.push(`${before}`)
			},
			//快乐8玩法选择器
			bindPickerChange (e) {
				this.gameTypeIndex = e.detail.value
				this.beforeNumberValue = this.gameTypeObj[this.gameTypeIndex].value
			},
			/**
			 * 初始化数组
			 * listLength：数组长度
			 * isAdd：是否加1(有些项目从1开始，有些从0开始)
			 */
			initList(listLength, isAdd = true) {
				let list = [];
				for (let i = 0; i < listLength; i++) {
					if (isAdd) {
						list[i] = i + 1;
					} else {
						list[i] = i;
					}
				}
				return list;
			},
			/**
			 * 递归函数
			 * list：目标数组
			 * num：选取几个数
			 * isDelete：使用过的数字是否删掉（有些项目数字可以重复的，例如七星彩）
			 * data：结果的数组，默认为[]
			 */
			setResultList(list, num, isDelete = true, data = []) {
				//多重复几遍，看运气
				let repeatRandom = Math.ceil(Math.random() * 10)
				let random = 0
				for (let i = 0; i < repeatRandom; i++) {
					random = parseInt(Math.random() * list.length)
				}
				if (repeatRandom < 4) {
					if (random === list.length - 1) {
						random = 0
					} else {
						random++
					}
				}
				if (repeatRandom > 7) {
					if (!random) {
						random = list.length - 1
					} else {
						random--
					}
				}
				
				data.push(list[random])
				isDelete && list.splice(random, 1)
				if (num === 1) {
					return data
				}
				return this.setResultList(list, --num, isDelete, data)
			},
			/**
			 * 组合数公式
			 * n!/(k!*(n - k)!)
			 */
			combination (n, k) {
				if (k === 0 || k === n) {
				    return 1;
				} else {
				    return this.combination(n - 1, k - 1) + this.combination(n - 1, k);
				}
			},
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		/* align-items: center; */
		justify-content: center;
		padding: 20upx 70upx;
	}

	.uni-padding-wrap {
		padding: 10px 30px;
	}
	.content-button {
		width: 50%;
		background-color: #007aff;
		color: #fff;
	}
	.content-text {
		height: 70vh;
		font-size: 38upx;
		overflow: auto;
	}

	.content-text-item {
		margin: 50upx 0;
	}

	.uni-icons {
		font-size: 35upx !important;
		padding-right: 20upx;
	}
</style>