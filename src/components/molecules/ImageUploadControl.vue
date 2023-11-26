<template>
  <div @dragover.prevent @drop="drop" draggable="true" @dragstart="dragStart">
    <div class="product">
      <ImagePreview :image="image" :alt-text="`Image ${index}`" />
      <button class="settings-button" @click.stop="toggleMenu">設定</button>

      <div class="product-menu" v-if="isOpen">
        <button @click="emitMove('left')">
          <img class="icon" src="~@/assets/images/left-arrow.png">
          左へ移動
        </button>
        <button @click="emitMove('right')">
          <img class="icon" src="~@/assets/images/right-arrow.png">
          右へ移動
        </button>
        <button class="warning" @click="emitRemove">
          <img class="icon" src="~@/assets/images/delete.svg">
          画像を削除
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import {defineComponent} from 'vue';
import ImagePreview from '@/components/atoms/ImagePreview.vue';

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

    return { toggleMenu, emitMove, emitRemove, dragStart, drop };
  }
});
</script>

<style scoped>
.warning {
  color: #ed4147 !important;
}

.icon {
  width: 25px;
  padding-right: 6px;
}

.product {
  background: #ffffff;
  border: 2px solid #e1e1e1;
  border-radius: 3px;
  overflow: hidden;
  position: relative;
  width: 237px;
  height: 250px;
  margin: 10px;
}

.settings-button {
  font-size: 19px;
  color: #f7f7f7;
  position: absolute;
  top: 4px;
  right: 4px;
  background: rgb(50, 50, 50);
  border: none;
  border-radius: 5%;
  width: 80px;
  height: 50px;
  text-align: center;
  opacity: 38%;
  cursor: pointer;
}

.settings-button:hover {
  background: rgb(0 0 0);
}

.product-menu {
  position: absolute;
  top: 1px;
  right: 2px;
  background: white;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  box-shadow: 2px 2px 5px rgba(0,0,0,0.2);
  width: 55%;
  height: 74%;
}

.product-menu button {
  width: 100%;
  border: none;
  border-bottom: 1px solid #d6d6d6; /* Apply to all initially */
  background: none;
  cursor: pointer;
  color: #333;
  font-size: 1.03em;
  display: flex;
  justify-content: flex-start;
  padding: 18px 0;
}

.product-menu button:last-child {
  border-bottom: none; /* Remove border for the last button */
}

.product-menu button:hover {
  background: #f0f0f0;
}

main {
  display: flex;
  flex-wrap: wrap;
}
</style>
