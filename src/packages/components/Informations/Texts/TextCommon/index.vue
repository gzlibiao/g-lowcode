<template>
  <div class="go-text-box">
    <div
      :style="`
      color: ${fontColor};
      padding: ${paddingY}px ${paddingX}px;
      font-size: ${fontSize}px;
      letter-spacing: ${letterSpacing}px;
      writing-mode: ${writingMode};

      border-style: solid;
      border-width: ${borderWidth}px;
      border-radius: ${borderRadius}px;
      border-color: ${borderColor};

      background-color:${backgroundColor}`"
    >
      {{ option.dataset }}
    </div>
  </div>
</template>
<script setup lang="ts">
import { PropType, toRefs, shallowReactive, watch } from 'vue'
import { CreateComponentType } from '@/packages/index.d'
import { useChartDataFetch } from '@/hooks'
import { useChartEditStore } from '@/store/modules/chartEditStore/chartEditStore'
import { option as configOption  } from './config'

const props = defineProps({
  chartConfig: {
    type: Object as PropType<CreateComponentType>,
    required: true
  }
})

const { w, h } = toRefs(props.chartConfig.attr)
const {
  dataset,
  fontColor,
  fontSize,
  letterSpacing,
  paddingY,
  paddingX,
  borderWidth,
  borderColor,
  borderRadius,
  writingMode,
  backgroundColor
} = toRefs(props.chartConfig.option)

const option = shallowReactive({
  dataset: configOption.dataset
})

// 手动更新
watch(
  () => props.chartConfig.option.dataset,
  (newData: any) => {
    option.dataset = newData
  }, {
    immediate: true
  }
)

// 预览更新
useChartDataFetch(props.chartConfig, useChartEditStore, (newData: string) => {
  option.dataset = newData
})
</script>

<style lang="scss" scoped>
@include go('text-box') {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
