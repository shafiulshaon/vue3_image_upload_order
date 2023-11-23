<template>
  <div class="main-template">
    <header>
      <!-- header content -->
    </header>
    <main>
      <!-- Slot for main content -->
      <slot></slot>
    </main>
    <footer>
      <!-- footer content -->
    </footer>
  </div>
</template>

<script>
export default {
  name: 'MainTemplate'
}
</script>

<style scoped>
.main-template header {
  /* header styles */
}
.main-template main {
  /* main content styles */
  padding: 20px;
  text-align: left;
}
.main-template footer {
  /* footer styles */
}
</style>
