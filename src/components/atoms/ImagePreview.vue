<template>
  <div className="image-preview">
    <img :src="image.url" :alt="altText"/>
  </div>
</template>

<script>
import {defineComponent, computed} from 'vue';

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
  setup(props) {
    const altTexts = computed(() => props.altText);

    return { altTexts };
  }
});
</script>

<style scoped>
.image-preview img {
  max-width: 100px;
  margin: 5px;
}
</style>
