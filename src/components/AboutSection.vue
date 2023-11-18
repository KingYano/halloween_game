<template>
  <div class="about__card">
    <div class="about__info">
        <div class="about__info--contact">
            <a target="_blank" href="https://github.com/KingYano"><i class="ri-github-line"></i></a>
            <a target="_blank" href="mailto:belkadi.yanis.l@gmail.com"><i class="ri-mail-send-line"></i></a>
            <a target="_blank" href="https://www.linkedin.com/in/yanis-belkadi-746727209/"><i class="ri-linkedin-line"></i></a>
        </div>
        <p class="about__legal-right">© 2023 BELKADI Yanis - Des bonbons ou un sort</p>
    </div>
  </div>
</template>

<script setup>

</script>

<style lang="scss">
    .about {
        &__card {
            height: 100%;
            width: 100%;
            padding: 15px;
        }

        &__legal-right {
            font-size: 0.6rem;
        }

        &__info {
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: space-between;

            &--contact {
                height: 80%;
                display: flex;
                flex-direction: column;
                gap: 15px;
                justify-content: center;
                align-items: center;

                a {
                    text-decoration: none;
                    border: 1px solid #ffe8ec;
                    border-radius: 50% 50%;
                    font-size: .6rem;
                    padding: 11px;

                    &:hover {
                        text-shadow: 0 2px 8px var(--first-color);
                    }
                }

                i {
                    color: var(--white-color);
                }
            }
        }
    }


    @media screen and (min-width: 1150px) {
        .about {
            &__info {
                height: 100%;

                &--contact {
                    height: 90%;
                    display: flex;
                    flex-direction: row;
                    gap: 80px;
                    justify-content: center;
                    align-items: center;

                    a {
                        font-size: 1rem;
                    }
                }
            }

            
        }
    }
</style>
