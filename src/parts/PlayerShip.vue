<template>
  <div
      class="player-ship"
      :style="playerStyle"
      @mousedown="(e) => handleMouseDown(e)"
  ></div>
</template>

<script>
export default {
  name: 'PlayerShip',
  props: {
    x: Number,
    y: Number,
    width: Number,
    height: Number
  },
  computed: {
    playerStyle () {
      return {
        left: `${this.x}px`,
        top: `${this.y}px`,
        width: `${this.width}px`,
        height: `${this.height}px`
      }
    }
  },
  methods: {
    handleMouseDown (e) {
      this.$emit('move', e.clientX > this.x ? 1 : -1)
    },
    /*handleTouchStart (e) {
      const touchX = e.touches[0].clientX
      this.$emit('move', touchX > this.x ? 1 : -1)
    }*/
  }
}
</script>

<style>
.player-ship {
  position: absolute;
  background-color: #3498db;
  transition: left 0.1s ease-out;
}
</style>