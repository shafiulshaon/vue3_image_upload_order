<template>
  <div>
    <input type="file" multiple @change="handleFileUpload" />
    <div v-if="images.length">
      <image-upload-control
        v-for="(image, index) in images"
        :key="image.url"
        :image="image"
        :index="index"
        @remove="removeImage"
      />
    </div>
    <button @click="uploadImages">Upload</button>
  </div>
</template>

<script>
import { ref } from 'vue';
import ImageUploadControl from '../molecules/ImageUploadControl.vue';

export default {
  components: {
    ImageUploadControl
  },
  setup() {
    const images = ref([]);

    const handleFileUpload = (event) => {
      const files = Array.from(event.target.files);
      images.value.push(...files.map(file => ({
        url: URL.createObjectURL(file),
        file: file
      })));
    };

    const removeImage = (index) => {
      images.value.splice(index, 1);
    };

    const uploadImages = () => {
      // Logic to upload images
    };

    return {
      images,
      handleFileUpload,
      removeImage,
      uploadImages
    };
  }
};
</script>

<!-- Add styles for the uploader if needed -->
