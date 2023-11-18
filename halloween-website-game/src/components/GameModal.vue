<template>
  <div class="modal__section">
    <div class="modal__close" v-on:click="closeModal">
        <i class="ri-close-line"></i>
    </div>
    <div class="modal__section--about"  v-if="props.section === 'about'">
      <AboutSection></AboutSection>
    </div>
    <div v-else class="modal__section--game">
      <GameSection></GameSection>
    </div>
  </div>
</template>

<script setup>
import GameSection from './GameSection.vue';
import AboutSection from './AboutSection.vue';

const emits = defineEmits(['close']);

const props = defineProps({
  section: String
});

const closeModal = () => {
    emits('close');
};
</script>

<style lang="scss">
.modal {
  &__section {
    position: absolute;
    top: 0;
    width: 100%;
    height: 100vh;
    background: rgba( 0, 0, 0, .5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 2;

    &--about {
      height: 250px;
      width: 500px;
      background: red;
    }

    &--game {
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
    }
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
