<template>
  <div class="game-status">
    Score: {{ score }}
  </div>
</template>

<script>
export default {
  name: 'GameStatus',
  props: {
    score: {
      type: Number,
      required: true
    }
  }
}
</script>

<style>
.game-status {
  position: absolute;
  top: 10px;
  left: 10px;
  color: white;
  font-size: 24px;
  z-index: 10;
}
</style>