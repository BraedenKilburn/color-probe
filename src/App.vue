<script setup>
import { computed, ref } from "vue";
import ControlForm from '@/components/ControlForm.vue';
import ImagePreview from './components/ImagePreview.vue';

const selectedImage = ref(null);

const handleSendImage = (image) => {
  selectedImage.value = image;
};

const eyeDropperSupported = computed(() => {
  return 'EyeDropper' in window;
});
</script>

<template>
  <div v-if="eyeDropperSupported" class="wrapper">
    <ControlForm @sendImage="handleSendImage" />
    <ImagePreview :image="selectedImage" />
  </div>
  <div v-else class="not-supported">
    <h1>Sorry, your browser does not support the EyeDropper API</h1>
    <p>You can
      <a
      href="https://developer.mozilla.org/en-US/docs/Web/API/EyeDropper#browser_compatibility"
      target="_blank">
        view browser capability here
      </a>
    </p>
  </div>
</template>

<style lang="scss">
.wrapper {
  display: grid;
  grid-template-columns: 450px 1fr;
  min-height: 100vh;

  color: white;
}

.not-supported {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;

  p {
    margin-top: 1rem;
    font-size: larger;
  }

  a {
    text-decoration: underline;
  }
}
</style>
