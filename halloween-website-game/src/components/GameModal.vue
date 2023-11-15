<template>
  <div class="modal__game">
    <div class="modal__close" v-on:click="closeModal">
        <i class="ri-close-line"></i>
    </div>
    <GameSection></GameSection>
  </div>
</template>

<script setup>
import GameSection from './GameSection.vue';
import { defineEmits  } from 'vue';

const emits = defineEmits(['close']);

const closeModal = () => {
    emits('close'); // Émettre l'événement
};
</script>

<style lang="scss">
.modal {
  &__game {
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

  &__close {
    position: absolute;
    top: 1rem;
    right: 1.5rem;
    font-size: 2rem;
    background-color: rgba(185, 185, 185, 0.5);
    border-radius: 10%;
    padding: 5px;
    cursor: pointer;
  }
}
</style>
