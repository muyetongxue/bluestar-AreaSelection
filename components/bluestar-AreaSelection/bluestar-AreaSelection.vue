<template>
	<view class="area-selection" v-show="show">
		<view :class="['content' , show ? 'show' : '']">
			<view class="subtitle">
				<button size="mini" style="margin: 0" @click="close">取消</button>
				<button size="mini" type="default" style="margin: 0" @click="sure">确定</button>
			</view>
			<picker-view style="width: 100%;height: 100%;" @change="areaChange">
				<picker-view-column>
					<view style="font-size: 32rpx;color: #333;line-height: 34px;text-align: center;">{{ topArea.areaName }}
					</view>
				</picker-view-column>
				<picker-view-column>
					<view style="font-size: 32rpx;color: #333;line-height: 34px;text-align: center;"
								v-for="(value,index) in areaList0.value"
								:key="index">{{ value.name }}
					</view>
				</picker-view-column>
				<picker-view-column>
					<view style="font-size: 32rpx;color: #333;line-height: 34px;text-align: center;"
								v-for="(value,index) in areaList1.value"
								:key="index">{{ value.name }}
					</view>
				</picker-view-column>
			</picker-view>
		</view>
	</view>
</template>

<script setup lang="ts">
import {onMounted, reactive, withDefaults} from "vue"
import {sysarealist} from "@/common/api/index"

const areaList0 : any = reactive({value : []})
const areaList1 : any = reactive({value : []})
let areaCode0 : string = ''
let areaCode1 : string = ''
let obj : object = {}

interface Props {
	show : boolean
	topArea : object | any
}

const props = withDefaults(defineProps<Props>(), {
	show : false,
	topArea : () => ({
		areaName : '',
		areaCode : '',
	}),
})

const emit = defineEmits<{
	(e : 'close') : void
	(e : 'sure', obj : object | any) : void
}>()

onMounted(() => init())

const init = async () => {
	const res0 : any = await sysarealist(Object.assign({}, {
		parentCode : props.topArea.areaCode,
	}))
	areaList0.value = res0.data
	areaCode0 = res0.data[0].areaCode

	const res1 : any = await sysarealist(Object.assign({}, {
		parentCode : areaCode0,
	}))
	areaList1.value = res1.data
	areaCode1 = res1.data[0].areaCode

	obj = Object.assign({}, {
		areaName : `${props.topArea.areaName}-${areaList0.value[0].name}-${areaList1.value[0].name}`,
		areaCode : areaCode1,
	})
}

//#ifdef H5
const areaChange = (event : any) => {
	if (!event.detail.value[1]) {
		return obj = Object.assign({}, {
			areaName : `${props.topArea.areaName}-${areaList0.value[0].name}-${areaList1.value[event.detail.value[2]].name}`,
			areaCode : areaCode1,
		})
	}

	if (areaList0.value[event.detail.value[1]].areaCode !== areaCode0) {
		areaCode0 = areaList0.value[event.detail.value[1]].areaCode
		sysarealist(Object.assign({}, {
			parentCode : areaCode0,
		})).then((res : any) => {
			if (res.code === 0 && res.data.length !== 0) {
				areaList1.value = res.data
				areaCode1 = res.data[0].areaCode
			} else {
				areaList1.value = []
				areaCode1 = areaCode0
			}
			if (event.detail.value.length === 2) {
				obj = Object.assign({}, {
					areaName : `${props.topArea.areaName}-${areaList0.value[event.detail.value[1]].name}-${areaList1.value[0].name}`,
					areaCode : areaCode1,
				})
			} else {
				obj = Object.assign({}, {
					areaName : `${props.topArea.areaName}-${areaList0.value[event.detail.value[1]].name}${areaList1.value.length !== 0 ? '-' + areaList1.value[event.detail.value[2]].name : ''}`,
					areaCode : areaCode1,
				})
			}
		})
	} else {
		obj = Object.assign({}, {
			areaName : `${props.topArea.areaName}-${areaList0.value[event.detail.value[1]].name}-${areaList1.value[event.detail.value[2]].name}`,
			areaCode : areaList1.value[event.detail.value[2]].areaCode,
		})
	}
}
//#endif

//#ifdef MP-WEIXIN
const areaChange = (event : any) => {
	if (areaList0.value[event.detail.value[1]].areaCode !== areaCode0) {
		areaCode0 = areaList0.value[event.detail.value[1]].areaCode
		sysarealist(Object.assign({}, {
			parentCode : areaCode0,
		})).then((res : any) => {
			if (res.code === 0 && res.data.length !== 0) {
				areaList1.value = res.data
				areaCode1 = res.data[0].areaCode
			} else {
				areaList1.value = []
				areaCode1 = areaCode0
			}
			obj = Object.assign({}, {
				areaName : `${props.topArea.areaName}-${areaList0.value[event.detail.value[1]].name}${areaList1.value.length !== 0 ? '-' + areaList1.value[event.detail.value[2]].name : ''}`,
				areaCode : areaCode1,
			})
		})
	} else {
		obj = Object.assign({}, {
			areaName : `${props.topArea.areaName}-${areaList0.value[event.detail.value[1]].name}-${areaList1.value[event.detail.value[2]].name}`,
			areaCode : areaList1.value[event.detail.value[2]].areaCode,
		})
	}
}
//#endif

//#ifdef APP-PLUS
const areaChange = (event : any) => {
	if (event.detail.value[1] !== null) {
		areaCode0 = areaList0.value[event.detail.value[1]].areaCode
		sysarealist(Object.assign({}, {
			parentCode : areaCode0,
		})).then((res : any) => {
			if (res.code === 0 && res.data.length !== 0) {
				areaList1.value = res.data
				areaCode1 = res.data[0].areaCode
			} else {
				areaList1.value = []
				areaCode1 = areaCode0
			}
			obj = Object.assign({}, {
				areaName : `${props.topArea.areaName}-${areaList0.value[event.detail.value[1]].name}${areaList1.value.length !== 0 ? '-' + areaList1.value[event.detail.value[2]].name : ''}`,
				areaCode : areaCode1,
			})
		})
	}
}
//#endif

const close = () => {
	emit('close')
}

const sure = () => {
	emit('sure', obj)
	close()
}

/*const con = () => {
	console.log(123)
}

defineExpose({con})*/
</script>

<style lang="scss" scoped>
.area-selection {
	position: fixed;
	top: 0;
	left: 0;
	z-index: 999;
	width: 100vw;
	height: 100vh;
	background-color: rgba(0, 0, 0, .3);
	.content {
		position: absolute;
		bottom: 0;
		left: 0;
		z-index: 1000;
		width: 100%;
		height: 40%;
		margin: 0;
		background-color: #ffffff;
		.subtitle {
			display: flex;
			justify-content: space-between;
			align-items: center;
			width: 100%;
			padding: vw(20);
			border-bottom: vw(2) solid #e1e1e1;
		}
		&.show {
			animation: showModal .4s;
		}
	}
}

@keyframes showModal {
	from {
		bottom: -50%;
	}
	to {
		bottom: 0;
	}
}
</style>
