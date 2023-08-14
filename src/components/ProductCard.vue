<template>
  <div 
    class="inline-flex w-14 h-14 items-center justify-center shadow-md cursor-pointer"
    draggable
    @dragstart="startDrag($event, color)"
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
  computed: {
    iconType() {
      return this.type === 'outer' ? 'fa-suitcase'
       : this.type === 'inner' ? 'fa-bag-shopping' : 'fa-wallet';
    }
  },
  methods: {
    startDrag(event, item) {
      event.dataTransfer.dropEffect = 'move';
      event.dataTransfer.effectAllowed = 'move';
      event.dataTransfer.setData('color', item);
    },
  }
}
</script>
