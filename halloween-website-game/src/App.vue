<template>
  <HeaderNavigation @gameClicked="toggleGameModal"></HeaderNavigation>
  <HomeSection></HomeSection>
  <GameModal v-if="showGameModal" @close="handleModalClose"></GameModal>
</template>

<script setup>
import HeaderNavigation from './components/HeaderNavigation.vue';
import HomeSection from './components/HomeSection.vue';
import GameModal from './components/GameModal.vue';
import { ref } from 'vue';

const showGameModal = ref(false);

const toggleGameModal = () => {
  showGameModal.value = !showGameModal.value;
  if (showGameModal.value) {
    document.body.style.overflowY = 'hidden';
  } else {
    document.body.style.overflowY = 'auto';
  }
};

const handleModalClose = () => {
  showGameModal.value = false;
  document.body.style.overflowY = 'auto';
};
</script>

<style lang="scss">
  @import './assets/style.scss';
</style>
