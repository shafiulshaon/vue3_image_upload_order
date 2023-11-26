<template>
  <div @dragover.prevent @drop="drop" draggable="true" @dragstart="dragStart">
    <div class="product">
      <ImagePreview :image="image" :alt-text="`Image ${index}`" />
      <Button class="settings-button" @click.stop="toggleMenu" title="設定"/>
      <div class="product-menu" v-if="isOpen">
        <Button @click="emitMove('left')" title="左へ移動">
          <img class="icon" :src="leftArrowIcon">
        </Button>
        <Button @click="emitMove('right')" title="右へ移動">
          <img class="icon" :src="rightArrowIcon">
        </Button>
        <Button class="warning" @click="emitRemove" title="画像を削除">
          <img class="icon" :src="deleteIcon">
        </Button>
      </div>
    </div>
  </div>
</template>

<script>
import {defineComponent} from 'vue';
import ImagePreview from '@/components/atoms/ImagePreview.vue';
import Button from '@/components/atoms/Button.vue';
import { leftArrowIcon, rightArrowIcon, deleteIcon } from '@/assets/icons';

export default defineComponent({
  components: {
    ImagePreview,
    Button
  },
  props: {
    image: {
      type: Object,
      required: true
    },
    index: {
      type: Number,
      required: true
    },
    isOpen: {
      type: Boolean,
      required: true
    },
  },
  emits: ['toggle-menu', 'remove', 'move', 'reorder'],
  setup(props, { emit }) {

    const toggleMenu = () => {
      emit('toggle-menu', props.index);
    };

    const emitMove = (direction) => {
      emit('move', { index: props.index, direction });
    };

    const emitRemove = () => {
      emit('remove', props.index);
    };

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

    return { toggleMenu, emitMove, emitRemove, dragStart, drop, leftArrowIcon, rightArrowIcon, deleteIcon };
  }
});
</script>

<style scoped>
</style>
