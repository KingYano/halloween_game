<template>
  <HeaderNavigation @sectionSelected="handleSectionSelected" @gameClicked="toggleGameModal"></HeaderNavigation>
  <HomeSection></HomeSection>
  <GameModal :section="currentSection" v-if="showGameModal" @close="handleModalClose"></GameModal>
</template>

<script setup>
  import HeaderNavigation from './components/HeaderNavigation.vue';
  import HomeSection from './components/HomeSection.vue';
  import GameModal from './components/GameModal.vue';
  import { ref, watch } from 'vue';

  const showGameModal = ref(false);
  const currentSection = ref('game');

  const toggleGameModal = (section = 'game') => {
    currentSection.value = section;
    showGameModal.value = !showGameModal.value;
  };

  const handleSectionSelected = (section) => {
    toggleGameModal(section);
  };

  const handleModalClose = () => {
    showGameModal.value = false;
  };

  const updateBodyStyle = () => {
    document.body.style.overflowY = showGameModal.value ? 'hidden' : 'auto';
  };

  watch(showGameModal, updateBodyStyle);
</script>

<style lang="scss">
  @import './assets/style.scss';
</style>
