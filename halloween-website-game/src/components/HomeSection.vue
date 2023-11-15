<template>
 <main class="main">
    <section class="home">
        <div class="home__container">
            <img src="../assets/image/home-point.png" alt="image" class="home__points">
            <img src="../assets/image/home-shadow.png" alt="image" class="home__shadow">
            <img src="../assets/image/home-moon.png" alt="image" class="home__moon">
            <img src="../assets/image/home-trees.png" alt="image" class="home__trees">
            <img src="../assets/image/home-pumpkin.png" alt="image" class="home__pumpkin">
            <img src="../assets/image/home-stones.png" alt="image" class="home__stones">
            <img src="../assets/image/home-grass.png" alt="image" class="home__grass">

            <div class="home__titles">
                <h2 class="home__subtitle">Des bonbons ou un sort</h2>
                <h1 class="home__title">HALLOWEEN</h1>
            </div>
        </div>

        <div class="home__data">
            <p class="home__description">
                Le jeu "Des bonbons ou un sort" est un jeu de course simple où le joueur contrôle un fantôme qui doit éviter des obstacles tels que des citrouilles et des pierres tombales. Le jeu gagne en vitesse et en difficulté au fil du temps.
            </p>

            <a v-on:click="handlePlayClick" class="home__button">
                <img src="../assets/image/button.svg" alt="image">
                <span>JOUER</span>
            </a>
        </div>
    </section>
    <GameModal v-if="showModal"></GameModal>
 </main>
</template>

<script setup>
import GameModal from './GameModal.vue'

// import { gsap } from 'gsap';
import { ref, onMounted } from 'vue';
const showModal = ref(false);
console.log('1', showModal)
function handlePlayClick() {
    console.log('mdr')
    showModal.value = true;
console.log('2', showModal)

}

onMounted(() => {
//   gsap.from('.home__shadow', 1.5, {opacity: 0, y: -300, delay: .2});
//   gsap.from('.home__points', 1.5, {opacity: 0, y: -300, delay: .4});
//   gsap.from('.home__grass', 1.5, {opacity: 0, y: 300, delay: .2, ease: 'elastic.out(1, .5)'});
//   gsap.from('.home__pumpkin', 1.5, {opacity: 0, y: -300, delay: .6, ease: 'bounce.out'});
//   gsap.from('.home__stones', 1.5, {opacity: 0, y: 300, delay: .8, ease: 'elastic.out(1, .5)'});
//   gsap.from('.home__moon', 1.5, {opacity: 0, y: 300, delay: 1.5, ease: 'elastic.out(1, .5)'});
//   gsap.from('.home__titles', 1.5, {opacity: 0, y: -300, delay: 1.5, ease: 'elastic.out(1, .5)'});
//   gsap.from('.home__trees', 1.5, {opacity: 0, y: 300, delay: 2, ease: 'elastic.out(1, .5)'});
//   gsap.from('.home__data', 1.5, {opacity: 0, y: 200, delay: 2, ease: 'elastic.out(1, .5)'});
});


</script>

<style lang="scss">
.home {
    position: relative;
    height: 100vh;
    background-color: var(--black-color);

    &__container {
        position: relative;
        height: 70%;
        background: var(--radial-color);
        display: grid;
        justify-items: center;
        overflow: hidden;

        img {
            position: absolute;
        }
    }

    &__grass, &__pumpkin {
        bottom: 0;
    }

    &__stones {
        bottom: -.5rem;
        width: 470px;
    }

    &__trees {
        bottom: .5rem;
        width: 470px;
    }

    &__moon {
        bottom: 2rem;
        width: 590px;
    }

    &__stones, &__trees, &__moon {
        max-width: initial;
    }

    &__shadow, &__points {
        top: 0%;
        left: 0%;
        width: 100%;
        height: 100%;
        object-fit: cover;
        object-position: center;
    }

    &__shadow {
        mix-blend-mode: overlay;
    }

    &__titles {
        position: absolute;
        bottom: 11rem;
        text-align: center;
    }

    &__subtitle, &__title {
        color: var(--black-color);
        font-family: var(--second-font);
        font-weight: var(--font-regular);
        text-shadow: 0 2px 8px var(--first-color);
    }

    &__subtitle {
        font-size: var(--big-font-size);
    }

    &__title {
        font-size: var(--biggest-font-size);
    }

    &__data {
        position: absolute;
        bottom: 4rem;
        text-align: center;
        margin-inline: 1.5rem;
    }

    &__description {
        margin-bottom: 2rem;
    }

    &__button {
        display: inline-grid;
        position: relative;
        place-items: center;
        transition: filter .4s;
        cursor: pointer;

        span {
            position: absolute;
            color: var(--white-color);
            font-size: var(--small-font-size);
            font-weight: var(--font-medium);
        }

        &:hover {
            filter: drop-shadow(0 4px 15px hsla(185, 100%, 48%, .6));
        }
    }
}

.game {
    padding-block: 150px;
    background: red;
    // height: 374px;
    // width: 864px;
    display: flex;
    justify-content: center;
    align-items: center;
}

// Small Screen
@media screen and (max-width: 330px) {
    .home {
        &__stones {
            width: 370px;
        }

        &__trees {
            width: 400px;
        }

        &__moon {
            width: 520px;
        }

        &__titles {
            bottom: 9rem;
        }

        &__data {
            bottom: 2rem;
        }
    }
}

// Medium Screen
@media screen and (min-width: 576px) {
    .home {
        &__container {
            height: 85%;
        }

        &__grass, &__stones, &__trees, &__moon {
            width: 100%;
        }

        &__moon {
            bottom: 3rem;
        }

        &__titles {
            bottom: 13rem;
        }

        &__data {
            bottom: 2rem;
        }
    }
}

@media screen and (min-width: 768px) {
    .home {
        &__titles {
            bottom: 16rem;
        }

        &__subtitle {
            font-size: 2.5rem;
        }

        &__title {
            font-size: 4.5rem;
        }
    }
}

// Large Screen
@media screen and (min-width: 968px) {
    .home {
        &__container {
            height: 90%;
        }

        &__moon {
            bottom: 0;
        }

        &__titles {
            bottom: 20rem;
        }

        &__data {
            max-width: 768px;
            left: 0;
            right: 0;
            margin-inline: auto;
        }
    }
}

@media screen and (min-width: 1150px) {
    .home {
        &__container {
            height: 100%;
        }

        &__titles {
            bottom: 25rem;
        }
    }
}

@media screen and (min-width: 1500px) {
    .home {
        height: initial;

        &__container {
            height: 1100px;
        }

        &__trees, &__moon {
            bottom: -8rem;
        }

        &__titles {
            bottom: 35rem;
        }

        &__subtitle {
            font-size: var(--big-font-size);
        }

        &__title {
            font-size: var(--biggest-font-size);
            line-height: 90%;
        }
    }
}

// Largest Screen > 2k
@media screen and (min-width: 2048px) {
    .home {
        &__container {
            height: 980px;
        }

        &__container {
            height: 1200px;
        }

        &__titles {
            bottom: 45rem;
        }
    }
}

</style>
