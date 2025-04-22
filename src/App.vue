<template>
  <div class="app">
    <GameBoard
        ref="gameBoard"
        @update-score="(newScore) => score = newScore"
        @game-over="gameOver = true"
        :game-over="gameOver"
        :score="score"
        @restart="restartGame"
    />
  </div>
</template>

<script>
import GameBoard from '@/components/GameBoard.vue'

export default {
  name: 'App',
  components: {
    GameBoard
  },
  data () {
    return {
      gameOver: false,
      score: 0
    }
  },
  methods: {
    restartGame () {
      /*this.gameOver = true // Сначала true для триггера
      this.$nextTick(() => {
        this.gameOver = false
        this.score = 0
      })*/
      this.gameOver = true
      this.score = -1 // Форсируем обновление

      this.$nextTick(() => {
        this.gameOver = false
        this.score = 0
        this.$refs.gameBoard.resetGame()
      })
    }
  }
}
</script>

<style>
.app {
  height: 100vh;
  overflow: hidden;
}
</style>
