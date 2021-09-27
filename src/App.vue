<template>
  <div v-if="!mobile" class="tags">
    <tag-input />
  </div>
  <div v-else class="mobile-message">
    <h2>Sorry, this app is not supported on mobile Devices</h2>
    <p>To use this app, please use a computer or Tablet</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import TagInput from './components/TagInput.vue'

const mobile = ref(false)

window.addEventListener('resize', checkScreen)
function checkScreen() {
  const windowWidth = window.innerWidth
  if (windowWidth <= 750) {
    mobile.value = true
    return
  }
  mobile.value = false
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@300;400;500;600&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Barlow Condensed', sans-serif;
}

.tags {
  padding-top: 160px;
  width: 100%;
}

.mobile-message {
  display: flex;
  height: 100vh;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: #000;
}

.mobile-message p {
  margin-top: 12px;
}
</style>
