<template>
  <div 
    class="inline-flex w-14 h-14 items-center justify-center shadow-md cursor-pointer"
    draggable
    @dragstart="handleDrag($event, color)"
    ref="draggableItem"
    @touchstart="handleTouchStart"
    @touchmove="handleTouchMove"
    @touchend="handleTouchEnd(color)"
  >
    <i :class="['fa-solid fa-2xl', iconType]" :style="`color: ${color};`"></i>
  </div>
</template>

<script>
export default {
  props: {
    type: { type: String, default: '' },
    color: { type: String, default: '' }
  },
  data() {
    return {
      dragging: false,
      initialX: 0,
      initialY: 0,
      currentX: 0,
      currentY: 0
    }
  },
  computed: {
    iconType() {
      return this.type === 'outer' ? 'fa-suitcase'
       : this.type === 'inner' ? 'fa-bag-shopping' : 'fa-wallet';
    }
  },
  methods: {
    // drag for computer
    handleDrag(event, item) {
      event.dataTransfer.dropEffect = 'move';
      event.dataTransfer.effectAllowed = 'move';
      event.dataTransfer.setData('color', item);
    },
    // touch for mobile
    handleTouchStart(event) {
      this.dragging = true;
      this.initialX = event.touches[0].clientX;
      this.initialY = event.touches[0].clientY;
    },
    handleTouchMove(event) {
      if (!this.dragging) return;
      event.preventDefault();
      this.currentX = event.touches[0].clientX - this.initialX;
      this.currentY = event.touches[0].clientY - this.initialY;
      this.changePosition();
    },
    handleTouchEnd(itemColor) {
      this.dragging = false;
      this.currentX = 0;
      this.currentY = 0;
      this.changePosition();
      this.$emit('phoneSelectProduct', itemColor)
    },
    changePosition() {
      const draggableItem = this.$refs.draggableItem;
      draggableItem.style.transform = `translate(${this.currentX}px, ${this.currentY}px)`;
    }
  }
}
</script>
