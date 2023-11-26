<template>
  <div v-if="images.length" class="container">
    <ImageUploadControl
      v-for="(image, index) in images"
      :key="index"
      :image="image"
      :index="index"
      :is-open="openMenuIndex === index"
      @toggle-menu="toggleMenu"
      @remove="removeImage"
      @move="moveImage"
      @reorder="reorderImages"
    />
  </div>
  <label for="file-upload" class="custom-file-upload">
    <img class="icon" :src="uploadFileIcon">
    ファイルを選択する
  </label>
  <input id="file-upload" type="file" multiple @change="handleFileUpload" >

  <Button v-if="images.length" class="upload-btn" @click="uploadImages" title="保存"/>
  <FeedbackModal
    :show="showModal"
    :loading="loading"
    :message="feedbackMessage"
    @close="showModal = false"
  />
</template>

<script>
import {ref, defineComponent, onBeforeUnmount} from 'vue';
import ImageUploadControl from '@/components/molecules/ImageUploadControl.vue';
import FeedbackModal from '@/components/organisms/FeedbackModal.vue';
import Button from '@/components/atoms/Button.vue';
import { uploadFileIcon } from '@/assets/icons';

export default defineComponent({
  name: 'ImageUploader',
  components: {
    ImageUploadControl,
    FeedbackModal,
    Button
  },
  setup() {
    const images = ref([]);
    const openMenuIndex = ref(-1);
    const showModal = ref(false);
    const loading = ref(false);
    const feedbackMessage = ref('');

    const toggleMenu = (index) => {
      if (openMenuIndex.value === index) {
        openMenuIndex.value = -1; // Close the menu if it's already open
      } else {
        openMenuIndex.value = index; // Open the new menu
        window.addEventListener('click', closeMenu);
      }
    };

    const closeMenu = (event) => {
      if (!event.target.matches('.settings-button')) {
        openMenuIndex.value = -1;
        window.removeEventListener('click', closeMenu);
      }
    };

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
      loading.value = true;
      showModal.value = true;
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
        feedbackMessage.value = '保存しました';
        console.log(result);
      } catch (error) {
        feedbackMessage.value = '失敗しました。: ' + error.message;
      } finally {
        loading.value = false;
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

    const moveImage = ({ index, direction }) => {
      const newPosition = direction === 'left' ? index - 1 : index + 1;
      if (newPosition >= 0 && newPosition < images.value.length) {
        const itemToMove = images.value.splice(index, 1)[0];
        images.value.splice(newPosition, 0, itemToMove);
      }
    };

    return {
      images,
      openMenuIndex,
      toggleMenu,
      handleFileUpload,
      removeImage,
      reorderImages,
      uploadImages,
      moveImage,
      showModal,
      loading,
      feedbackMessage,
      uploadFileIcon
    };
  }
});
</script>

<style scoped>
.container {
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;
  padding: 10px;
}
.icon {
  width: 15px;
  padding-right: 5px;
}
</style>