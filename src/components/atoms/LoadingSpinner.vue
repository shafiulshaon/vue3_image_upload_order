<template>
  <div class="spinner"></div>
</template>

<script>
export default {
  name: 'LoadingSpinner'
};
</script>

<style scoped>
.spinner {
  border: 4px solid rgba(0,0,0,0.1);
  width: 36px;
  height: 36px;
  border-radius: 50%;
  border-left-color: #15cda6;
  animation: spin 1s ease infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
