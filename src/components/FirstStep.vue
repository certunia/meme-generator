<script setup>
import {ref, getCurrentInstance} from "vue";

const imgs = ref([
  'same_picture.webp',
  'same_picture.webp',
  'same_picture.webp',
  'drake.jpeg',
  'head.jpeg',
  'sup.jpeg',
  'me_and_boys.png',
]);

const { emit } = getCurrentInstance();

const chooseImg = (e) => {
  emit('chooseImg', e.target.src);
}
</script>

<template>
  <div class="template-gallery">
    <div v-for="img in imgs" :key="img" @click="chooseImg">
      <img :src="'memes/' + img" :alt="img">
    </div>
  </div>
</template>

<style scoped>
.template-gallery {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
  width: 100%;
}
</style>
