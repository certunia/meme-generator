<script setup lang="ts">
import FirstStep from './components/FirstStep.vue'
import Canvas from './components/Canvas.vue'
import {ref} from "vue";

const step = ref(1);

const log = (e) => {
  console.log('e');
  console.log(e);

  step.value = 2;
}
</script>

<template>
  <div class="content">
    <FirstStep v-if="step === 1" class="full" @chooseImg="log"/>
    <Canvas v-if="step === 2" class="full"/>
  </div>
</template>

<style scoped>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  padding: 40px;
}
</style>
