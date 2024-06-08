<template lang="pug">
.py-4.px-4
  .flex.flex-col.gap-2.text-lg
    .flex.items-center.justify-center.gap-1
      .w-8.font-mono.text-pink-600 VO
      input.input-box.flex-1(ref="input_vo" type="number" :tabindex="11" v-model="data.vo" @focus="autoChoose"
       @keydown="handleKeydown($event,input_da,input_vi)")
    .flex.items-center.justify-center.gap-1
      .w-8.font-mono.text-blue-600 DA
      input.input-box.flex-1(ref="input_da" type="number" :tabindex="12" v-model="data.da" @focus="autoChoose"
      @keydown="handleKeydown($event,input_vi,input_vo)")
    .flex.items-center.justify-center.gap-1
      .w-8.font-mono.text-orange-300 VI
      input.input-box.flex-1(ref="input_vi" type="number" :tabindex="13" v-model="data.vi" @focus="autoChoose"
      @keydown="handleKeydown($event,input_vo,input_da)")
  .py-2.pl-10.opacity-75
    .mb-3.flex
      input.mr-1(type="checkbox" id="-bonus" name="-bonus" v-model="bonus") 
      label.text-base(for="-bonus") 奖励属性加成(各属性+30)
    .flex.gap-2.text-base
      div 三围合计:
      .mr-5.font-mono {{ sum }}
      div 魔法数值:
      .font-mono {{ st_value }}
  .my-2.text-2xl.flex.flex-col.gap-2.font-mono
    .flex.gap-2
      .w-8.text-right S
      .text-right(class="w-[4em]") {{ targetScore.po_s_value | 0}}
    .flex.gap-2
      .w-8.text-right A+
      .text-right(class="w-[4em]") {{ targetScore.po_ap_value | 0}}
    .flex.gap-2
      .w-8.text-right A
      .text-right(class="w-[4em]") {{ targetScore.po_a_value | 0}}

  .bg-white.w-full.my-8(class="h-[1px]")
  .font-black.mb-4 评价分反推决赛分数
  .flex.gap-2.mb-2.items-center
    .w-28 目标评价分:
    input.input-box.flex-1(type="number" min="0" v-model="points" @focus="autoChoose")
  .flex.gap-2.mb-10
    .w-28 决赛分数:
    .w-28.font-mono.text-xl {{ targetState | 0}}

  NuxtLink(to="/about") 参考
</template>
<script setup lang="ts">
import { computed, reactive, ref, watch,onMounted } from "vue";
definePageMeta({
  title:'学mas计算器'
})
const input_vo = ref<any>(null)
const input_da = ref<any>(null)
const input_vi = ref<any>(null)
const data = reactive<any>({
  vo: 500,
  da: 500,
  vi: 500
})
const bonus = ref(true);
onMounted(() => {
  input_vo.value?.focus();
})
watch(data,()=>{
  for(let key in data){
    if(data[key]>1500){
      setTimeout(()=>data[key]=1500,300)
    }
    if(data[key]<0){
      setTimeout(()=>data[key]=0,300)
    }
  }
})
const sum = computed(() => {
  let { vo, da, vi } = data;
  vo = vo || 0;
  da = da || 0;
  vi = vi || 0;
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

function handleKeydown(ev:any,next:any,prev:any){
  if(ev.key === "ArrowDown"){
    next?.focus();
    ev.preventDefault();
  }
  if(ev.key === "ArrowUp"){
    prev?.focus();
    ev.preventDefault();
  }
  if(ev.key === "Enter"){
    next?.focus();
    ev.preventDefault();
  }
}
function autoChoose(ev: any) {
  ev.target.select()
}
</script>
<style lang="scss">

.input-box{
  @apply bg-black text-neutral-50 px-2 py-1 text-lg rounded-md border border-transparent 
  focus:border-blue-400 outline-none transition-all font-mono;
}
</style>