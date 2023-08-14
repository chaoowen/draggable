<template>
  <span
    :class="[
      'py-2 px-3 bg-primary text-white rounded-full cursor-pointer',
      isChecked ? 'bg-primary' : 'bg-secondary'
    ]"
    @click="handleClick"
  >
    <span>{{ text }}</span>
  </span>
</template>

<script>
export default {
  props: {
    text: { type: String, default: '' },
    isChecked: { type: Boolean, default: false }
  },
  methods: {
    handleClick() {
      this.$emit('handleClick')
    }
  }
}
</script>
