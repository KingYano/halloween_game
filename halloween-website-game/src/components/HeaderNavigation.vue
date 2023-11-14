<template>
  <header class="header" id="header">
    <nav class="nav container">
        <a href="#" class="nav__logo">
            Halloween
        </a>

        <div class="nav__menu" :class="isOpen === true ? 'show-menu' : ''" id="nav-menu">
            <ul class="nav__list">
                <li v-for="link in navLinks" :key="link.text" class="nav__item">
                    <a href="#" v-on:click="closeNav" class="nav__link">{{ link.text }}</a>
                </li>
            </ul>

            <div class="nav__close" v-on:click="closeNav" id="nav-close">
                <i class="ri-close-line"></i>
            </div>
        </div>

        <div class="nav__toggle" v-on:click="toggleNav" id="nav-toggle">
            <i class="ri-apps-2-line"></i>
        </div>
  </nav>
  </header>
</template>

<script setup>

import { ref } from 'vue';

// Menu Navigation
const isOpen = ref(false);
const navLinks = [
    {text: 'Accueil'},
    {text: 'À Propos'},
    {text: 'Jeu'},
    {text: 'Contact'},
]

const toggleNav = () => {
    isOpen.value =!isOpen.value;
    console.log('isOpen action', isOpen)
}

const closeNav = () => {
    isOpen.value = false;
};

</script>

<style lang="scss">
// Desktop Navigation
.header {
    position: fixed;
    width: 100%;
    top: 0%;
    left: 0%;
    background: transparent;
    z-index: var(--z-fixed);
    transition: background-color .3s;

    .show-menu {
        top: 0;
    }
    .nav {
        height: var(--header-height);
        display: flex;
        justify-content: space-between;
        align-items: center;

        &__logo{
            color: var(--white-color);
            font-family: var(--second-font);
            font-size: var(--h2-font-size);
            transition: text-shadow .3s;

            &:hover {
                text-shadow: 0 2px 8px var(--first-color);
            }

        }

        &__toggle, &__close {
            display: flex;
            font-size: 1.25rem;
            cursor: pointer;
        }
    }
}

// Responsive Navigation
@media screen and (max-width: 1150px) {
    .nav {
        &__menu {
            position: fixed;
            top: -100%;
            left: 0;
            background-color: var(--black-color);
            width: 100%;
            padding-block: 5rem 4.5rem;
            transition: top .5s;
        }

        &__list {
            display: flex;
            flex-direction: column;
            text-align: center;
            row-gap: 3rem;
        }

        &__item {

        }

        &__link {
            color: var(--white-color);
            font-weight: var(--font-medium);
            transition: text-shadow .3s;

            &:hover {
                text-shadow: 0 2px 8px var(--first-color);
            }
        }

        &__close {
            position: absolute;
            top: 1rem;
            right: 1.5rem;
        }
    }
}

</style>
