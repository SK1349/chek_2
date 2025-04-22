<template>
  <div
      class="star"
      :style="starStyle"
  ></div>
</template>

<script>
export default {
  name: 'Star',
  props: {
    x: {
      type: Number,
      required: true
    },
    y: {
      type: Number,
      required: true
    },
    size: {
      type: Number,
      required: true
    }
  },
  computed: {
    starStyle () {
      return {
        left: `${this.x}px`,
        top: `${this.y}px`,
        width: `${this.size}px`,
        height: `${this.size}px`
      }
    }
  }
}
</script>

<style>
.star {
  position: absolute;
  background-color: gold;
  border-radius: 50%;
}
</style>