<template>
    <div class="game-container" ref="gameContainer">
        <div class="game-info">
            <div v-if="!isGameOver && gameStarted" id="scoreDisplay">{{ score }}</div>
            <button class="game-info__pause" id="pauseButton" v-if="!isGameOver && gameStarted" @click="togglePause">{{ isPaused ? 'Reprendre' : 'Pause' }}</button>
        </div>
        <div class="start-text" id="loading-message" v-if="!gameStarted">Appuyez sur Espace <span class="game-icon"><i class="ri-space"></i></span> ou la flèche vers le haut <span class="game-icon"><i class="ri-arrow-up-line"></i></span> pour commencer</div>
        <div class="game-over">
            <button class="game-over__button" id="restartButton" @click="restartGame" v-if="isGameOver">Relancer</button>
            <h2 class="game-over__title" id="gameOverMessage" v-if="isGameOver">GAME OVER</h2>
            <div v-if="isGameOver" id="scoreDisplay">score : {{ score }}</div>
        </div>
        <div id="cube" :style="{ bottom: cubeBottom + 'px' }"></div>
        <div id="obstacles">
            <div v-for="obstacle in obstacles" :key="obstacle.id" :class="['obstacle', obstacle.type, { 'paused': isPaused }]" :style="{ left: obstacle.left + 'px', height: obstacle.height, width: obstacle.width }"></div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';

const gameContainer = ref(null);
const gameStarted = ref(false);
const isPaused = ref(false);
const isGameOver = ref(false);
const score = ref(0);
const obstacles = ref([]);
const cubeBottom = ref(0);
let scoreInterval, obstacleSpeed = 5, obstacleCount = 0, jumping = false, startTime, pauseTime, duration = 800;

const startGame = () => {
    gameStarted.value = true;
    scoreInterval = setInterval(updateScore, 250);
    requestAnimationFrame(gameLoop);
};

const jump = () => {
    if (!jumping && !isPaused.value && !isGameOver.value) {
        jumping = true;
        startTime = performance.now();
        requestAnimationFrame(updateJump);
    }
};

const updateJump = (currentTime) => {
    if (!isPaused.value) {
        const elapsedTime = currentTime - startTime;
        const progress = elapsedTime / duration;
        const height = -4 * (progress - 0.5) * (progress - 0.5) + 1;
        cubeBottom.value = height * 100;
        if (progress < 1) {
            requestAnimationFrame(updateJump);
        } else {
            jumping = false;
            cubeBottom.value = 0;
        }
    }
};

const createObstacle = () => {
    if (!isPaused.value && !isGameOver.value) {
        const isPumpkin = Math.random() < 0.5;
        const obstacle = {
            id: obstacleCount++,
            left: gameContainer.value.clientWidth,
            height: isPumpkin ? '30px' : '41px',
            width: isPumpkin ? '34px' : '27px',
            type: isPumpkin ? 'pumpkin' : 'funerary-stone'
        };
        obstacles.value.push(obstacle);
    }
};

let lastObstacleCreationTime = 0;
const obstacleCreationInterval = 2100;

let lastFrameTime = 0;
const frameRate = 60;
const frameInterval = 1000 / frameRate;

const gameLoop = (currentTime) => {
    const delta = currentTime - lastFrameTime;
    if (delta > frameInterval) {
        if (!isPaused.value && !isGameOver.value) {
            moveObstacles();
            if (currentTime - lastObstacleCreationTime > obstacleCreationInterval) {
                createObstacle();
                lastObstacleCreationTime = currentTime;
            }
        }
        lastFrameTime = currentTime - (delta % frameInterval);
    }
    requestAnimationFrame(gameLoop);
};


const moveObstacles = () => {
    obstacles.value.forEach(obstacle => {
        obstacle.left -= obstacleSpeed;
        checkCollision(obstacle);
        if (obstacle.left <= -125) {
            obstacles.value = obstacles.value.filter(o => o.id !== obstacle.id);
        }
    });
};

const checkCollision = (obstacle) => {
    const cubeRect = { left: 50, right: 83, top: cubeBottom.value, bottom: cubeBottom.value + 36 };
    const obstacleRect = { left: obstacle.left, right: obstacle.left + 34, top: 0, bottom: parseInt(obstacle.height) };
    if (cubeRect.left < obstacleRect.right && cubeRect.right > obstacleRect.left && cubeRect.bottom > obstacleRect.top && cubeRect.top < obstacleRect.bottom) {
        gameOver();
    }
};

const updateScore = () => {
    score.value++;
    if (score.value % 100 === 0) {
        obstacleSpeed += 1;
        clearInterval(scoreInterval);
        scoreInterval = setInterval(updateScore, 250 - score.value / 100);
    }
};

const gameOver = () => {
    isGameOver.value = true;
    isPaused.value = true;
    clearInterval(scoreInterval);
};

const restartGame = () => {
    isPaused.value = false;
    isGameOver.value = false;
    jumping = false;
    cubeBottom.value = 0;
    obstacles.value = [];
    score.value = 0;
    obstacleSpeed = 5;
    obstacleCount = 0;
    startGame();
};

const togglePause = () => {
    if (!isGameOver.value) {
        isPaused.value = !isPaused.value;
        if (isPaused.value) {
            pauseTime = performance.now();
            clearInterval(scoreInterval);
        } else {
            if (jumping) {
                startTime += performance.now() - pauseTime;
                requestAnimationFrame(updateJump);
            }
            scoreInterval = setInterval(updateScore, 250);
        }
    }
};

onMounted(() => {
    window.addEventListener('keydown', (e) => {
        if (e.code === 'Space' || e.code === 'ArrowUp') {
            if (!gameStarted.value) {
                startGame();
            } else {
                jump();
            }
        }
    });
});

watch(isGameOver, (newVal) => {
    if (newVal) {
        clearInterval(scoreInterval);
    }
});
</script>

<style lang="scss">
.game-container {
    position: relative;
    width: 55%;
    height:40%;
    background: #02254b;
    overflow: hidden;
}

#cube {
    position: absolute;
    width: 33px;
    height: 36px;
    border-radius: 23%;
    background-image: url('../assets/image/game/ghost.png');
    background-size: cover;
    bottom: 0;
    left: 50px;
}

.obstacle {
    position: absolute;
    bottom: 0;
    right: 0;
}

.pumpkin {
    background-image: url('../assets/image/game/pumpkin.png');
    background-size: cover;
    border-radius: 40%;
}

.funerary-stone {
    background-image: url('../assets/image/game/funerary-stone.png');
    background-size: cover;
}

.game-over {
    display: flex;
    flex-direction: column;
    height: 75%;
    align-items: center;
    justify-content: center;
    grid-gap: 15px;
    padding: 35px;

    &__button {
        font-size: 1rem;
        padding: 5px;
        border-radius: 50px;
        cursor: pointer;
    }

    &__title {
        color: var(--white-color);
        font-family: var(--second-font);
        font-size: var(--h2-font-size);
        letter-spacing: 6px;
    }

}

.game-info {
    padding: 8px;
    display: flex;
    justify-content: space-between;

    &__pause {
        padding: 4px;
        border-radius: 50px;
        cursor: pointer;
    }
}

.game-icon {
    border: solid 1px;
}

.start-text {
    text-align: center;
    padding: 25px;
    font-size: 1rem;
    line-height: 1.5;
    animation-duration: 1.5s;
    animation-name: clignoter;
    animation-iteration-count: infinite;
    transition: none;
}

@keyframes clignoter {
  0%   { opacity:1; }
  40%   {opacity:0; }
  100% { opacity:1; }
}
</style>
