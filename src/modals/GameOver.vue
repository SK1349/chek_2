<template>
  <div class="game-over">
    <div class="game-over__content">
      <h1 class="game-over__title">Game Over</h1>
      <p class="game-over__score">Score: {{ score }}</p>
      <button
          class="game-over__button"
          @click="$emit('restart')">
        Play Again
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GameOver',
  props: {
    score: {
      type: Number,
      required: true
    }
  },
  emits: ['restart']
}
</script>

<style>
.game-over {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.8);
  z-index: 100;
}

.game-over__content {
  background-color: #111;
  padding: 30px;
  border-radius: 10px;
  text-align: center;
  color: white;
}

.game-over__button {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 10px 20px;
  margin-top: 15px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}
</style>