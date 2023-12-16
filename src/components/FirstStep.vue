<script setup>
import 'vue3-carousel/dist/carousel.css'
import { Carousel, Slide, Pagination, Navigation } from 'vue3-carousel'
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
  <carousel :items-to-show="1.5">
    <slide v-for="img in imgs" :key="img" @click="chooseImg">
      <img :src="'memes/' + img" :alt="img">
    </slide>

    <template #addons>
      <navigation />
      <pagination />
    </template>
  </carousel>
</template>

<style scoped>
.carousel__item {
  width: 100%;
  background-color: var(--vc-clr-primary);
  color: var(--vc-clr-white);
  font-size: 20px;
  border-radius: 8px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.carousel__slide {
  max-height: 94vh;
  padding: 10px;
}

.carousel__slide img {
  max-width: 50vw;
}

.carousel__prev,
.carousel__next {
  box-sizing: content-box;
  border: 5px solid white;
}
</style>
