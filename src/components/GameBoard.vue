<template>
  <div
      class="game-board"
      ref="gameContainer"
  >
    <GameStatus :score="score" />

    <Player
        :x="player.x"
        :y="player.y"
        :width="player.width"
        :height="player.height"
        @move="(direction) => handlePlayerMove(direction)"
    />

    <Star
        v-for="(star, index) in stars"
        :key="'star'+index"
        :x="star.x"
        :y="star.y"
        :size="starSize"
    />

    <Asteroid
        v-for="(asteroid, index) in asteroids"
        :key="'asteroid'+index"
        :x="asteroid.x"
        :y="asteroid.y"
        :size="asteroidSize"
    />

    <GameOverModal
        v-if="gameOver"
        :score="score"
        @restart="$emit('restart')"
    />

    <GameControls
        @move-left="moveLeft"
        @move-right="moveRight"
        @stop-move="stopMove"
    />
  </div>
</template>

<script>
import PlayerShip from '@/parts/PlayerShip.vue'
import StarObject from '@/parts/StarObject.vue'
import AsteroidObject from '@/parts/AsteroidObject.vue'
import GameControls from '@/parts/GameControls.vue'
import GameStatus from '@/parts/GameStatus.vue'
import GameOver from '@/modals/GameOver.vue'

export default {
  name: 'GameBoard',
  components: {
    Player: PlayerShip,
    Star: StarObject,
    Asteroid: AsteroidObject,
    GameControls,
    GameStatus,
    GameOverModal: GameOver
  },
  props: {
    gameOver: {
      type: Boolean,
      required: true
    },
    score: {
      type: Number,
      default: 0
    }
  },
  data () {
    return {
      gameWidth: 0,
      gameHeight: 0,
      player: {
        x: 0,
        y: 0,
        width: 40,
        height: 40,
        speed: 5,
        moving: 0
      },
      stars: [],
      asteroids: [],
      starSize: 20,
      asteroidSize: 30,
      gameLoop: null,
      starSpawnRate: 60,
      asteroidSpawnRate: 120,
      starSpawnCounter: 0,
      asteroidSpawnCounter: 0
    }
  },
  mounted () {
    this.initGame()
    window.addEventListener('resize', this.handleResize)
  },
  beforeDestroy () {
    cancelAnimationFrame(this.gameLoop)
    window.removeEventListener('resize', this.handleResize)
  },
  methods: {
    initGame () {
      this.handleResize()
      this.resetGame()
      this.startGame()
    },
    handleResize () {
      const container = this.$refs.gameContainer
      if (container) {
        this.gameWidth = container.clientWidth
        this.gameHeight = container.clientHeight
        this.player.y = this.gameHeight - this.player.height - 60
        this.player.x = (this.gameWidth - this.player.width) / 2
      }
    },
    resetGame () {
      this.stars = []
      this.asteroids = []
      this.$emit('update:score', 0)
      this.$emit('update:game-over', false)
      this.handleResize()
      this.startGame()
    },
    startGame () {
      cancelAnimationFrame(this.gameLoop)
      this.gameLoop = requestAnimationFrame(this.update)
    },
    update () {
      if (this.gameOver) return

      if (this.player.moving !== 0) {
        this.player.x += this.player.speed * this.player.moving

        if (this.player.x < 0) this.player.x = 0
        if (this.player.x > this.gameWidth - this.player.width) {
          this.player.x = this.gameWidth - this.player.width
        }
      }

      this.starSpawnCounter++
      if (this.starSpawnCounter >= this.starSpawnRate) {
        this.spawnStar()
        this.starSpawnCounter = 0
      }

      this.asteroidSpawnCounter++
      if (this.asteroidSpawnCounter >= this.asteroidSpawnRate) {
        this.spawnAsteroid()
        this.asteroidSpawnCounter = 0
      }

      for (let i = this.stars.length - 1; i >= 0; i--) {
        this.stars[i].y += this.stars[i].speed

        if (this.checkCollision(this.player, this.stars[i])) {
          this.$emit('update-score', this.score + 10);
          this.stars.splice(i, 1)
          continue
        }

        if (this.stars[i].y > this.gameHeight) {
          this.stars.splice(i, 1)
        }
      }

      for (let i = this.asteroids.length - 1; i >= 0; i--) {
        this.asteroids[i].y += this.asteroids[i].speed

        if (this.checkCollision(this.player, this.asteroids[i])) {
          this.$emit('game-over');
          //this.$emit('update:game-over', true)
          break
        }

        if (this.asteroids[i].y > this.gameHeight) {
          this.asteroids.splice(i, 1)
        }
      }

      this.gameLoop = requestAnimationFrame(this.update)
    },
    spawnStar () {
      this.stars.push({
        x: Math.random() * (this.gameWidth - this.starSize),
        y: -this.starSize,
        speed: 2 + Math.random() * 3,
        width: this.starSize,
        height: this.starSize
      })
    },
    spawnAsteroid () {
      this.asteroids.push({
        x: Math.random() * (this.gameWidth - this.asteroidSize),
        y: -this.asteroidSize,
        speed: 3 + Math.random() * 4,
        width: this.asteroidSize,
        height: this.asteroidSize
      })
    },
    checkCollision (obj1, obj2) {
      return obj1.x < obj2.x + obj2.width &&
          obj1.x + obj1.width > obj2.x &&
          obj1.y < obj2.y + obj2.height &&
          obj1.y + obj1.height > obj2.y
    },
    handlePlayerMove (direction) {
      this.player.moving = direction
    },
    moveLeft () {
      this.player.moving = -1
    },
    moveRight () {
      this.player.moving = 1
    },
    stopMove () {
      this.player.moving = 0
    }
  }
}
</script>

<style>
.game-board {
  position: relative;
  width: 100%;
  height: 100%;
  background-color: #111;
  overflow: hidden;
}
</style>