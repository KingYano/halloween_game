<template>
    <div class="game-container" ref="gameContainer">
        <div id="loading-message" v-show="!gameStarted">Appuyez sur Espace ou la flèche vers le haut pour commencer</div>
        <div id="pauseButton" @click="togglePause">{{ isPaused ? 'Reprendre' : 'Pause' }}</div>
        <div id="scoreDisplay">{{ score }}</div>
        <button id="restartButton" @click="restartGame" v-show="isGameOver">Relancer</button>
        <div id="cube" :style="{ bottom: cubeBottom + 'px' }"></div>
        <div id="obstacles">
            <div v-for="obstacle in obstacles" :key="obstacle.id" :class="['obstacle', obstacle.type, { 'paused': isPaused }]" :style="{ left: obstacle.left + 'px', height: obstacle.height }"></div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, watch, nextTick } from 'vue';

const gameContainer = ref(null);
const gameStarted = ref(false);
const isPaused = ref(false);
const isGameOver = ref(false);
const score = ref(0);
const obstacles = ref([]);
const cubeBottom = ref(0);
let gameInterval, scoreInterval, obstacleSpeed = 2, obstacleCount = 0, jumping = false, startTime, pauseTime, duration = 800;

const startGame = () => {
    gameStarted.value = true;
    gameInterval = setInterval(createObstacle, 2500);
    scoreInterval = setInterval(updateScore, 100);
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
        const obstacle = { id: obstacleCount++, left: gameContainer.value.clientWidth, height: Math.random() < 0.5 ? '30px' : '50px', type: Math.random() < 0.5 ? 'pumpkin' : 'funerary-stone' };
        obstacles.value.push(obstacle);
        nextTick(() => {
            moveObstacle(obstacle);
        });
    }
};

const moveObstacle = (obstacle) => {
    const move = setInterval(() => {
        if (!isPaused.value) {
            obstacle.left -= obstacleSpeed;
            checkCollision(obstacle);
            if (obstacle.left <= -125) {
                clearInterval(move);
                obstacles.value = obstacles.value.filter(o => o.id !== obstacle.id);
            }
        }
    }, 5);
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
    if (score.value % 150 === 0) {
        obstacleSpeed++;
    }
};

const gameOver = () => {
    isGameOver.value = true;
    isPaused.value = true;
    clearInterval(gameInterval);
    clearInterval(scoreInterval);
};

const restartGame = () => {
    isPaused.value = false;
    isGameOver.value = false;
    jumping = false;
    cubeBottom.value = 0;
    obstacles.value = [];
    score.value = 0;
    obstacleSpeed = 2;
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
            scoreInterval = setInterval(updateScore, 100);
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
        clearInterval(gameInterval);
        clearInterval(scoreInterval);
    }
});
</script>

<style lang="scss">
.game-container {
    position: relative;
    width: 60%;
    height:40%;
    background: #87CEEB;
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
    width: 34px;
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
</style>
