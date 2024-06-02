<template lang="pug">
.py-4.px-4.text-lg
  .grid.grid-cols-3.mb-5.gap-4
    .flex.items-center.justify-center.gap-1
      .text-pink-600 VO:
      InputNumber.flex-1(min="0" max="1500" :tabindex="11" v-model:value="data.vo" autofocus @focus="autoChoose")
    .flex.items-center.justify-center.gap-1
      .text-blue-600 DA:
      InputNumber.flex-1(min="0" max="1500" :tabindex="12" v-model:value="data.da" @focus="autoChoose")
    .flex.items-center.justify-center.gap-1
      .text-orange-300 VI:
      InputNumber.flex-1(min="0" max="1500" :tabindex="13" v-model:value="data.vi" @focus="autoChoose")
  .py-3
    Checkbox(v-model:checked="bonus") 决赛第一奖励属性加成(各属性+30)
  .my-5.pt-3.text-lg.flex.flex-col.gap-2
    .flex.gap-2.text-base
      .w-28 三围合计:
      .w-28 {{ sum }}
      .w-28 魔法数值:
      .w-28 {{ st_value }}
    .flex.gap-2
      .w-28 S目标分数:
      .w-28 {{ targetScore.po_s_value}}
    .flex.gap-2
      .w-28 A+目标分数:
      .w-28 {{ targetScore.po_ap_value}}
    .flex.gap-2
      .w-28 A目标分数:
      .w-28 {{ targetScore.po_a_value}}

  .bg-white.w-full.my-4(class="h-[1px]")
  .font-black.mb-3 评价反推决赛分数
  .flex.gap-2.mb-4
    .w-28 目标评价分:
    InputNumber.flex-1(min="0" v-model:value="points" @focus="autoChoose")
  .flex.gap-2.mb-4
    .w-28 决赛分数:
    .w-28 {{ targetState }}

</template>
<script setup lang="ts">
import { computed, reactive, ref } from "vue";
import { InputNumber, Checkbox } from 'ant-design-vue';
const data = reactive({
  vo: 500,
  da: 500,
  vi: 500
})
const bonus = ref(true)
const sum = computed(() => {
  let { vo, da, vi } = data;
  if (bonus.value) {
    vo = Math.min(1500, vo + 30)
    da = Math.min(1500, da + 30)
    vi = Math.min(1500, vi + 30)
  };
  return vo + da + vi;
})
const st_value = computed(() => {
  return Math.floor(sum.value * 23 / 10)
})

const targetScore = computed(() => {

  const husoku_s = 13000 - 1700 - st_value.value;
  let po_s_value = 0, po_ap_value = 0, po_a_value = 0;
  if (husoku_s < 1500) {
    po_s_value = husoku_s / 0.3;
  } else if (husoku_s < 2250) {
    po_s_value = 5000 + (husoku_s - 1500) / 0.15;
  } else if (husoku_s < 3050) {
    po_s_value = 10000 + (husoku_s - 2250) / 0.08;
  } else if (husoku_s < 3450) {
    po_s_value = 20000 + (husoku_s - 3050) / 0.04;
  } else if (husoku_s < 3650) {
    po_s_value = 30000 + (husoku_s - 3450) / 0.02;
  } else {
    po_s_value = 40000 + (husoku_s - 3650) / 0.01;
  }
  const husoku_ap = 11500 - 1700 - st_value.value;
  if (husoku_ap < 1500) {
    po_ap_value = husoku_ap / 0.3;
  } else if (husoku_ap < 2250) {
    po_ap_value = 5000 + (husoku_ap - 1500) / 0.15;
  } else if (husoku_ap < 3050) {
    po_ap_value = 10000 + (husoku_ap - 2250) / 0.08;
  } else if (husoku_ap < 3450) {
    po_ap_value = 20000 + (husoku_ap - 3050) / 0.04;
  } else if (husoku_ap < 3650) {
    po_ap_value = 30000 + (husoku_ap - 3450) / 0.02;
  } else {
    po_ap_value = 40000 + (husoku_ap - 3650) / 0.01;
  }
  const husoku_a = 10000 - 1700 - st_value.value;
  if (husoku_a < 1500) {
    po_a_value = husoku_a / 0.3;
  } else if (husoku_a < 2250) {
    po_a_value = 5000 + (husoku_a - 1500) / 0.15;
  } else if (husoku_a < 3050) {
    po_a_value = 10000 + (husoku_a - 2250) / 0.08;
  } else if (husoku_a < 3450) {
    po_a_value = 20000 + (husoku_a - 3050) / 0.04;
  } else if (husoku_a < 3650) {
    po_a_value = 30000 + (husoku_a - 3450) / 0.02;
  } else {
    po_a_value = 40000 + (husoku_a - 3650) / 0.01;
  }
  return {
    po_s_value,
    po_ap_value,
    po_a_value
  }
})

const points = ref(10000)
const targetState = computed(() => {
  const husoku = points.value - 1700 - st_value.value;
  if (husoku < 1500) {
    return husoku / 0.3;
  } else if (husoku < 2250) {
    return 5000 + (husoku - 1500) / 0.15;
  } else if (husoku < 3050) {
    return 10000 + (husoku - 2250) / 0.08;
  } else if (husoku < 3450) {
    return 20000 + (husoku - 3050) / 0.04;
  } else if (husoku < 3650) {
    return 30000 + (husoku - 3450) / 0.02;
  } else {
    return 40000 + (husoku - 3650) / 0.01;
  }

})


function autoChoose(ev: any) {
  ev.target.select()
}
</script>
<style scoped lang="scss"></style>