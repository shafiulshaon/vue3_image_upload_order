<template>
  <MainTemplate>
    <ImageUploader />
  </MainTemplate>
</template>

<script>
import MainTemplate from '@/templates/MainTemplate.vue';
import ImageUploader from '@/components/organisms/ImageUploader.vue';

export default {
  name: 'HomePage',
  components: {
    MainTemplate,
    ImageUploader
  }
}
</script>
