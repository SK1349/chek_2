<template>
  <div class="game-controls">
    <button
        class="game-controls__button game-controls__button--left"
        @mousedown="$emit('move-left')"
        @mouseup="$emit('stop-move')"
        @mouseleave="$emit('stop-move')"
    >←</button>
    <button
        class="game-controls__button game-controls__button--right"
        @mousedown="$emit('move-right')"
        @mouseup="$emit('stop-move')"
        @mouseleave="$emit('stop-move')"
    >→</button>
  </div>
</template>

<script>
export default {
  name: 'GameControls',
  emits: ['move-left', 'move-right', 'stop-move']
}
</script>

<style>
.game-controls {
  position: absolute;
  bottom: 10px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 20px;
  padding: 10px;
}

.game-controls__button {
  background-color: #2c3e50;
  color: white;
  border: none;
  padding: 15px 25px;
  font-size: 20px;
  border-radius: 5px;
  cursor: pointer;
  user-select: none;
}

.game-controls__button:active {
  background-color: #1a252f;
}
</style>