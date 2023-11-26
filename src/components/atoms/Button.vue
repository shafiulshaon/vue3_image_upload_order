<template>
  <button :class="btnType" @click="onClick">
    <slot></slot>
    {{ title }}
  </button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    title: String,
    btnType: {
      type: String,
      default: 'btn'
    }
  },
  emits: ['click'],
  methods: {
    onClick(event) {
      this.$emit('click', event);
    }
  }
};
</script>

<style scoped>
</style>
