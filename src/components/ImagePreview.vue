<script setup>
import { defineProps, ref, computed } from 'vue';

const props = defineProps(['image']);
const src = computed(() => props.image || NoImage);
</script>

<template>
  <div className="image-preview">
    <div v-if="props.image" class="image-container">
      <img :src="props.image" alt="Working image" />
      <div
        class="background-image"
        :style="{ backgroundImage: `url(${props.image})` }"
      />
    </div>
    <div v-else>
      <svg fill="currentColor" xmlns="http://www.w3.org/2000/svg" height="48" viewBox="0 -960 960 960" width="48">
        <path d="M452-202h60v-201l82 82 42-42-156-152-154 154 42 42 84-84v201ZM220-80q-24 0-42-18t-18-42v-680q0-24 18-42t42-18h361l219 219v521q0 24-18 42t-42 18H220Zm331-554v-186H220v680h520v-494H551ZM220-820v186-186 680-680Z"/>
      </svg>
    </div>
  </div>
</template>

<style lang="scss">
.image-preview {
  background: #363844;
  padding: 2.4rem;
  display: grid;
  place-content: center;
  height: 100vh;
  box-sizing: border-box;
  position: relative;
  overflow: hidden;

  .image-container {
    display: flex;
    align-items: center;
    justify-content: center;

    img {
      max-width: 90%;
      max-height: 90%;
      object-fit: contain;
      box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.15);
      position: relative;
      z-index: 10;
    }

    .background-image {
      position: absolute;
      inset: 0;
      z-index: 0;

      background-size: cover;
      background-position: center;
      transform: scale(1.2);

      filter: blur(4px);
      opacity: 0.5;
    }
  }
}
</style>