<template>
  <div>
    <input type="file" multiple @change="handleFileUpload" />
    <div v-if="images.length">
      <ImageUploadControl
      v-for="(image, index) in images"
      :key="index"
      :image="image"
      :index="index"
      @remove="removeImage"
      @reorder="reorderImages"
    />
    </div>
    <button @click="uploadImages">Upload</button>
  </div>
</template>

<script>
import {ref, defineComponent, onBeforeUnmount} from 'vue';
import ImageUploadControl from '../molecules/ImageUploadControl.vue';

export default defineComponent({
  components: {
    ImageUploadControl
  },
  setup() {
    const images = ref([]);

    const handleFileUpload = (event) => {
      const files = Array.from(event.target.files).filter(file => file.type.startsWith('image/'));

      if (files.length === 0) {
        // No images selected, show an error message
        return;
      }

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


    const reorderImages = ({ originIndex, targetIndex }) => {
      const origin = images.value[originIndex];
      images.value.splice(originIndex, 1);
      images.value.splice(targetIndex, 0, origin);
    };

    onBeforeUnmount(() => {
      images.value.forEach(image => URL.revokeObjectURL(image.url));
    });

    return {
      images,
      handleFileUpload,
      removeImage,
      reorderImages,
      uploadImages
    };
  }
});
</script>

<!-- styles for the uploader -->
