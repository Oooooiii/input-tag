<template>
  <div class="tag-input">
    <h1>Tag input</h1>
    <div class="tag-main">
      <div>
        <div v-for="(tag, index) in tags" :key="index" class="tag-input__tag">
          <span @click="removeTag(index)">
            <icons-close />
            <span>{{ tag }}</span>
          </span>
        </div>
        <input
          type="text"
          placeholder="Enter a tag"
          class="tag-input__text"
          @keydown="addTag"
          @keydown.delete="removeLastTag"
        />
      </div>
      <div v-if="isTyping">
        <span>Typing...</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watchEffect } from 'vue'
import IconsClose from '~icons/mdi/close'

const tags = ref([])
const isTyping = ref(false)

watchEffect((onInvalidate) => {
  if (tags.value.length > 0) {
    isTyping.value = true

    const showTypingStatus = setTimeout(() => {
      isTyping.value = false
    }, 2000)

    onInvalidate(() => {
      clearInterval(showTypingStatus)
    })
  }
})

function addTag(event) {
  if (event.code == 'Comma' || event.code == 'Enter') {
    event.preventDefault()
    var val = event.target.value.trim()

    if (val.length > 0) {
      tags.value.push(val)
      event.target.value = ''
    }
  }
}

function removeTag(index) {
  tags.value.splice(index, 1)
}

function removeLastTag(event) {
  if (event.target.value.length === 0) {
    removeTag(tags.value.length - 1)
  }
}
</script>

<style scoped>
.tag-input {
  border: 1px solid #eee;
  font-size: 0.9rem;
  height: auto;
  box-sizing: border-box;
  padding: 0 10px;
  margin: 0 64px;
}
.tag-main {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.tag-main div:nth-child(2) {
  opacity: 0.8;
  padding-right: 8px;
  align-self: flex-end;
  padding-bottom: 16px;
}
.tag-input h1 {
  margin: 2px 0;
}
.tag-input__tag {
  height: 30px;
  float: left;
  margin-right: 10px;
  background-color: #eee;
  margin-top: 10px;
  line-height: 30px;
  padding: 0 5px;
  border-radius: 5px;
}
.tag-input__tag span {
  cursor: pointer;
  opacity: 0.8;
  display: flex;
  align-items: center;
}
.tag-input__tag span svg {
  margin-right: 2px;
}
.tag-input__text {
  border: none;
  outline: none;
  font-size: 0.9rem;
  line-height: 50px;
  background: none;
}
</style>
