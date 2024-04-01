<script setup lang="ts">
import { ref } from 'vue'

let active = ref(false)
let toggleActive = () => {
  active.value = !active.value
}

let image = ref({ name: '', url: '' })

let onFileSelect = (event) => {
  let files = event.target.files
  if (files[0].type.split('/'[0] === 'image')) {
    image.value = { name: files[0].name, url: URL.createObjectURL(files[0]) }
  }
}

const imageInput = ref<null | { click: () => null }>(null)
let imageInputClick = () => {
  imageInput.value?.click()
}

let onDragOver = (event) => {
  event.dataTransfer.dropEffect = 'copy'
}

let onDrop = (event) => {
  let files = event.dataTransfer.files
  if (files[0].type.split('/'[0] === 'image')) {
    image.value = { name: files[0].name, url: URL.createObjectURL(files[0]) }
  }
}
</script>

<template>
  <div
    @dragenter.prevent="toggleActive"
    @dragleave.prevent="toggleActive"
    @dragover.prevent="onDragOver"
    @drop.prevent="onDrop"
    class="dropzone"
    :class="{ 'active-dropzone': active }"
  >
    <img :src="image.url" @click="imageInputClick" v-show="image.url" />
    <div class="empty" v-show="!image.url">
      <span>Перетащите</span>
      <span>или</span>
      <label for="imageInput">Выберите файл</label>
      <input
        type="file"
        id="imageInput"
        ref="imageInput"
        class="dropzoneFile"
        accept="image/png, image/jpg, image/jpeg"
        @change="onFileSelect"
      />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.dropzone {
  width: 400px;
  height: 200px;
  color: #000;
  background-color: #fff;
  transition: 0.3s ease all;
  border: 2px dashed #41b883;

  img {
    height: 100%;
    width: 100%;
  }

  .empty {
    min-height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    row-gap: 16px;

    label {
      padding: 8px 12px;
      color: #fff;
      background-color: #41b883;
      transition: 0.3s ease all;
    }

    input {
      display: none;
    }
  }
}

.active-dropzone {
  color: #fff;
  border-color: #fff;
  background-color: #41b883;

  .empty {
    label {
      background-color: #fff;
      color: #41b883;
    }
  }
}
</style>
