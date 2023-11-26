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
    <img class="icon" src="~@/assets/images/upload-file.svg">
    ファイルを選択する
  </label>
  <input id="file-upload" type="file" multiple @change="handleFileUpload" >

  <button v-if="images.length" class="upload-btn" @click="uploadImages">保存</button>
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
    const openMenuIndex = ref(-1);

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
      moveImage
    };
  }
});
</script>

<style scoped>
.container {
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;
  margin: 0 auto;
  padding: 10px;
}
.icon {
  width: 15px;
  padding-right: 5px;
}
input[type="file"] {
  display: none;
}
.custom-file-upload {
  width: 140px;
  display: flex;
  justify-content: center;
  padding: 2px 6px;
  cursor: pointer;
  font-size: 0.8em;
  border-radius: 4px;
  border: 2px solid #a5a5a5;
  background-color: #e3e3e3;
  margin: 20px;
}

.upload-btn {
  width: 50%;
  align-self: center;
  height: 35px;
  color: white;
  background-color: #05d5ac;
  border: none;
  margin-top: 35px;
}
</style>