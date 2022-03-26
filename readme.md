# bluestar-AreaSelection

## 市县乡三级区划选择

## 引入

在script标签中引入组件

```typescript
//使用HBuilderX导入插件
import AreaSelection from "@/uni_modules/bluestar-AreaSelection/components/bluestar-AreaSelection/bluestar-AreaSelection.vue"

//使用npm install
import AreaSelection from "bluestar-areaselection/components/bluestar-AreaSelection/bluestar-AreaSelection.vue"
```

## 代码演示

```vue

<template>
	<AreaSelection :show="show" :topArea="topArea" @close="close" @sure="sure"></AreaSelection>
</template>

<script setup lang="ts">
import {reactive, ref} from "vue"

const show = ref(false)

const topArea : any = reactive({
	areaName : '仙桃市',
	areaCode : '429004',
})

const close = () => {
	show.value = false
}

const sure = (obj : object) => {
	console.log('areaCode:' + obj)
}
</script>
```

## API

#### Props

| 参数   | 说明             | 类型           | 默认值/参考值                                |
|------|:---------------|:-------------|:---------------------------------------|
| show | 是否显示弹出层        | `boolean`    | false                                  |
| topArea | 顶级地市区域名称/编码    | `object`     | {"areaName":"仙桃市","areaCode":"429004"} |

#### Events

| 事件名称 | 说明        | 回调参数 |
| :----- |:----------|-----|
| bing:close | 点击取消按钮时触发 | -   |
| bind:sure | 点击确定按钮时触发 | 选中值 |

## 作者

![](https://img.shields.io/static/v1?label=蓝星软件&message=@caisheng&labelColor=0E75FC)
