<script setup>
import { onBeforeUnmount, computed, ref } from 'vue';

const image = ref(null);
const emit = defineEmits(['sendImage'])
const handleFileInput = (e) => {
  image.value = URL.createObjectURL(e.target.files[0]);
  emit('sendImage', image.value);
};

onBeforeUnmount(() => {
  URL.revokeObjectURL(image.value);
});

const buttonBackgroundColor = ref('#000000');
const openEyeDropper = async () => {
  let eyeDropper = new EyeDropper();
  const { sRGBHex } = await eyeDropper.open();
  buttonBackgroundColor.value = sRGBHex;
};

const buttonTextColor = computed(() => {
  // Change button text color based on background color
  const hex = buttonBackgroundColor.value.replace('#', '');
  const r = parseInt(hex.substring(0, 2), 16);
  const g = parseInt(hex.substring(2, 4), 16);
  const b = parseInt(hex.substring(4, 6), 16);
  const yiq = (r * 299 + g * 587 + b * 114) / 1000;
  return yiq >= 128 ? '#000000' : '#ffffff';
});

const copied = ref(false);
const handleCopyColor = async () => {
  await navigator.clipboard.writeText(buttonBackgroundColor.value);
  copied.value = true;

  setTimeout(() => {
    copied.value = false;
  }, 2000);
};
</script>

<template>
  <div class="control-form">
    <h1 class="heading-text">Probe color from image</h1>

    <div class="form-section">
      <p>1. Select an image</p>
      <input @change="handleFileInput" type="file" accept="image/*" />
    </div>

    <div class="form-section">
      <p>2. Pick color</p>
      <button class="open-picker-button" @click="openEyeDropper">
        Open Eyedropper
      </button>
    </div>

    <div class="form-section">
      <p>3. View selected</p>
      <button
        class="selected-color"
        :style="{ background: buttonBackgroundColor, color: buttonTextColor }"
        @click="handleCopyColor"
      >
        <span>{{ buttonBackgroundColor }}</span>
      </button>
      <p class="copied-message" v-if="copied">&#x2705; Copied!</p>
    </div>
    <span class="shoutout">
      Made with ❤️ by 
      <a href="https://github.com/BraedenKilburn" target="_blank">
        @braedenkilburn
      </a>
    </span>
  </div>
</template>

<style lang="scss">
.control-form {
  width: 100%;
  height: 100vh;

  background: #22232b;
  padding: 1.6rem;

  position: sticky;
  top: 0;
}

.heading-text {
  margin: 0;
  margin-bottom: 1.6rem;
}

.form-section {
  margin-bottom: 1.6rem;

  p {
    font-size: 1rem;
    margin-bottom: 0.6rem;
    color: #ededf1;
  }

  .copied-message {
    text-align: center;
    background: rgb(0 128 0 / 50%);
    border-radius: 25px;
    padding: 20px;
    width: fit-content;
    margin: 20px auto;
  }
}

.open-picker-button {
  width: 100%;

  background: #6c738b;
  color: white;
  border: none;

  font-size: 1rem;
  padding: 0.8em 0.8em;
  border-radius: 0.2rem;

  cursor: pointer;

  transition: 0.2s background;

  &:hover {
    background: #46495e;
  }
}

.selected-color {
  width: 100%;
  height: 150px;

  border: none;
  cursor: pointer;
  border-radius: 0.4rem;

  transition: 0.2s background;

  display: grid;
  place-items: center;
  color: white;
  font-size: 1.2rem;
}

.shoutout {
  color: #6c738b;

  position: absolute;
  bottom: 1.6rem;
  left: 1.6rem;

  a {
    color: #b59cfd;
  }
}
</style>