<template>
  <div class="modal-game">
    <GameSection></GameSection>
  </div>
</template>

<script setup>
import GameSection from './GameSection.vue';

</script>

<style lang="scss">
.modal-game {
    position: absolute;
    top: 0;
    width: 100%;
    height: 100%;
    background: rgba( 0, 0, 0, .5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 2;
}
</style>
