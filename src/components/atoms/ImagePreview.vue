<template>
  <img class="img-preview" :src="image.url" :alt="altText"/>
</template>

<script>
import {defineComponent} from 'vue'

export default defineComponent({
  props: {
    image: {
      type: Object,
      required: true
    },
    altText: {
      type: String,
      default: 'Uploaded image'
    }
  },
})
</script>

<style scoped>
.img-preview {
  width: 100%;
  height: 100%;
  object-fit: scale-down;
  display: block;
}
</style>
