<template lang="pug">
ConfigProvider(:theme="appTheme")
  Calc
</template>
<script setup>
import { ConfigProvider, theme } from 'ant-design-vue';
import Calc from "@/components/Calc.vue";

const appTheme = {
  algorithm: theme.darkAlgorithm,
  token:{
    fontSize:16
  }
}
</script>
<style scoped></style>