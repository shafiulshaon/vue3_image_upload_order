<template>
  <Modal :show="show">
    <div v-if="loading">
      <LoadingSpinner />
    </div>
    <div v-else>
      <p>{{ message }}</p>
      <button class="close-btn" @click="close">閉じる</button>
    </div>
  </Modal>
</template>

<script>
import Modal from '@/components/atoms/Modal.vue'
import LoadingSpinner from '@/components/atoms/LoadingSpinner.vue'

export default {
  name: 'FeedbackModal',
  components: {
    Modal,
    LoadingSpinner
  },
  props: {
    show: Boolean,
    loading: Boolean,
    message: String
  },
  emits: ['close'],
  methods: {
    close() {
      this.$emit('close')
    }
  }
}
</script>

<style scoped>
</style>