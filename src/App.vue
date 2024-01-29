<script setup lang="ts">
import { onMounted, ref } from 'vue';

const audio = new Audio('/assets/sound/hover.ogg');

const mounted = ref(false);

const volume = ref(20);

const hoverText = ref('Egamesy radio');

const radioMenu = [
    { image: 'egamesy', title: 'Egamesy radio' },
    { image: 'rc', title: 'Radio centras' },
    { image: 'pwr', title: 'Power hit radio' },
    { image: 'pwrgold', title: 'Power gold' },
    { image: 'm1', title: 'M-1' },
    { image: 'zipfm', title: 'Zip fm' },
    { image: 'rusradio', title: 'Rus radio' },
    { image: 'santafm', title: 'Santa FM' },
];

function getItemStyle(index: number) {
    const totalItems = radioMenu.length;
    const angle = (index / totalItems) * 2 * Math.PI;
    const x = Math.cos(angle) * 11;
    const y = Math.sin(angle) * 11;
    return {
        transform: `translate(${x}rem, ${y}rem)`,
    };
}

function handleItemClick(item: (typeof radioMenu)[0]) {
    // play radio  volume.value
    console.log('Clicked:', item.title);
}

function changeVolume() {
    // change volume   volume.value
    console.log('volume changed to: ' + volume.value);
}

onMounted(() => {
    audio.volume = 0.5;
    setTimeout(() => {
        mounted.value = true;
    }, 100);
});
</script>

<template>
    <div class="radio-control">
        <div class="circular-menu">
            <div
                v-for="(item, index) in radioMenu"
                :key="index"
                class="menu-item"
                :style="mounted === true && getItemStyle(index)"
                @click="handleItemClick(item)"
                @mouseenter="
                    hoverText = item.title;
                    audio.play();
                "
                @mouseleave="
                    hoverText = '';
                    audio.pause();
                    audio.currentTime = 0;
                "
            >
                <img class="radio-images" :src="`/assets/images/${item.image}.png`" alt="" />
            </div>

            <div class="menu-item">Išjungti</div>
            <div class="hover-text" v-if="hoverText.length > 0">
                {{ hoverText }}
            </div>
        </div>
        <div class="volume-container">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white" width="1rem">
                <path
                    d="M5,9V15H9L14,20V4L9,9M18.5,12C18.5,10.23 17.5,8.71 16,7.97V16C17.5,15.29 18.5,13.76 18.5,12Z"
                />
            </svg>
            <input id="volume-slider" v-model="volume" @change="changeVolume" type="range" />
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="white" width="1rem">
                <path
                    d="M14,3.23V5.29C16.89,6.15 19,8.83 19,12C19,15.17 16.89,17.84 14,18.7V20.77C18,19.86 21,16.28 21,12C21,7.72 18,4.14 14,3.23M16.5,12C16.5,10.23 15.5,8.71 14,7.97V16C15.5,15.29 16.5,13.76 16.5,12M3,9V15H7L12,20V4L7,9H3Z"
                />
            </svg>
        </div>
    </div>
</template>

<style scoped>
.radio-control {
    width: 100vw;
    height: 100vh;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-weight: bold;
    color: whitesmoke;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    overflow: hidden;
    user-select: none;
}

.circular-menu {
    width: 100%;
    height: 50%;
    display: grid;
    place-items: center;
}

.menu-item {
    position: absolute;

    background-color: rgba(0, 0, 0, 0.5);
    width: 5rem;
    height: 5rem;
    border-radius: 50%;
    display: grid;
    place-items: center;
    opacity: 0.5;
    transition: all 0.3s ease;
}

.menu-item:hover {
    opacity: 1;
    cursor: pointer;
    scale: 1.01;
}

.hover-text {
    margin-top: 8rem;
}

.radio-images {
    width: 80%;
}

.volume-container {
    margin-top: 2rem;
    display: flex;
    align-items: center;
}

#volume-slider {
    appearance: none;
    height: 0.2rem;
    width: 20rem;
    border-radius: 5px;
    background-color: rgb(49, 49, 49);
}

#volume-slider::-webkit-slider-thumb {
    appearance: none;
    background-color: whitesmoke;
    width: 0.3rem;
    height: 1rem;
    border-radius: 20px;
    border: 1px solid rgb(49, 49, 49);
    cursor: pointer;
}
</style>
