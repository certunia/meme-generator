<script setup lang="ts">
import FirstStep from './components/FirstStep.vue'
import {ref} from "vue";

const step = ref(1);

const log = (e) => {
  console.log('e');
  console.log(e);
}
</script>

<template>
  <div class="content">
    <FirstStep v-if="step === 1" class="full" @chooseImg="log"/>
  </div>
</template>

<style scoped>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  max-width: 80%;
  margin: 0 auto;
}
</style>
