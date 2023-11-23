<template>
  <div
    class="image-upload-control"
    draggable="true"
    @dragstart="dragStart"
    @dragover.prevent
    @drop="drop"
  >
    <ImagePreview :image="image" :alt-text="`Image ${index}`" />
    <button @click="emitRemove">Remove</button>
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import ImagePreview from '../atoms/ImagePreview.vue';

export default defineComponent({
  components: {
    ImagePreview
  },
  props: {
    image: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    }
  },
  emits: ['remove', 'reorder'],
  setup(props, { emit }) {
    const dragStart = (event) => {
      event.dataTransfer.effectAllowed = 'move';
      event.dataTransfer.setData('application/vnd.myapp.index', props.index);
    };

    const drop = (event) => {
      const originIndex = event.dataTransfer.getData('application/vnd.myapp.index');
      const targetIndex = props.index;
      if (originIndex !== targetIndex.toString()) {
        emit('reorder', { originIndex, targetIndex });
      }
    };

    const emitRemove = () => {
      emit('remove', props.index);
    };

    return { emitRemove, dragStart, drop };
  }
});
</script>

<!-- styles for .image-upload-control -->
