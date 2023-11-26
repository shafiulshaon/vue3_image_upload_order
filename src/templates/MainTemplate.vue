<template>
  <div class="main-template">
    <header>
      <!-- header content -->
    </header>
    <main>
      <Aside />
      <section>
          <slot></slot>
      </section>
    </main>
    <footer>
      <!-- footer content -->
    </footer>
  </div>
</template>

<script>
import Aside from "@/components/organisms/aside/index"
import {defineComponent} from "vue"
export default defineComponent({
  name: 'MainTemplate',
  components: {
    Aside
  },
})
</script>

<style scoped>
.main-template header {
  /* header styles */
}
.main-template main {
  display: flex;
  flex-wrap: nowrap;
  justify-content: center;
  min-width: calc(100vw - 20px);
}

.main-template section {
  display: flex;
  flex-direction: column;
  min-height: calc(100vh - 120px);
  width: 75%;
}
.main-template footer {
  /* footer styles */
}
</style>
