<template lang="pug" functional>
div.square(
  :class="{ active: props.active }"
  :style="{ width: props.width, height: props.width }"
)
</template>

<script>
export default {
  name: 'Square',
  props: {
    active: {
      type: Boolean,
      required: true
    },
    width: {
      type: String,
      required: true
    }
  }
}
</script>

<style lang="scss" scoped>
.square {
  background: transparent;
  &.active {
    background: #000;
  }
}
</style>

