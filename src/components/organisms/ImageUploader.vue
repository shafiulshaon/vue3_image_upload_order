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
import ImageUploadControl from '@/components/molecules/ImageUploadControl.vue';

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

    const uploadImages = async () => {
      try {
        // sending the images as FormData
        const formData = new FormData();
        images.value.forEach((image, index) => {
          formData.append(`images[${index}]`, image.file, image.file.name);
        });

        // Update the URL with API endpoint
        const response = await fetch('https://httpbin.org/anything', {
          method: 'POST',
          body: formData,
        });

        const result = await response.json();
        // Handle the response data
        console.log(result);
      } catch (error) {
        // Handle any errors, such as by displaying a user-friendly message
        console.error('Upload failed:', error);
      }
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
