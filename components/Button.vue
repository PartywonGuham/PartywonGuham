<template>
    <button
        @click="handleClick()"
        @mousedown="e => handleMousedown(e, color)"
        @mouseup="e => handleMouseup(e, color)"
        @mouseleave="e => handleMouseup(e, color)"
        v-bind:class="`w-full h-full p-2 flex justify-center items-center gap-3 shrink-0 rounded-lg border-2 outline-none duration-200 
                        ${
                            color === 'orange' 
                            ? `${fill ? 'bg-orange-f6 lg:bg-orange-f6 text-white': 'text-orange-f6 lg:hover:bg-orange-f6 lg:hover:text-white'} border-orange-f6`
                            : `${fill ? 'bg-gray-4 text-white' : 'text-gray-4 lg:hover:bg-gray-4 lg:hover:text-white'} border-gray-4`
                        }
                    `"
    > 
        <i
            v-if="icon !== ''"
            v-bind:class="`${icon}`"
        ></i>
        <slot v-if="!isLoading">{{ text }}</slot>
        <div v-if="isLoading" class="loader w-5 h-5 border-2 border-white"></div>
    </button>
</template>

<script setup lang="ts">
    const props = defineProps({
        icon: {
            type: String,
            required: false,
            default: ""
        },
        color: {
            type: String,
            required: false,
            default: "orange"
        },
        fill: {
            type: Boolean,
            required: false,
            default: false,
        },
        isLoading: {
            type: Boolean,
            required: false,
            default: false,
        },
        text: String,
        click: Function,
    });

    const handleClick = props.click ? props.click : () => { return }

    // 마우스 이벤트
    const handleMousedown = (e: MouseEvent, color: string) => {
        const button = e.currentTarget as HTMLButtonElement;
        if (color === "orange") {
            button.classList.remove("lg:bg-orange-f6");
            button.classList.add("lg:bg-orange-f3", "lg:border-orange-f3");
        } else {
            button.classList.remove("lg:hover:bg-gray-4");
            button.classList.add("lg:hover:bg-gray-6", "lg:border-gray-6");
        }
    }
    const handleMouseup = (e: MouseEvent, color: string) => {
        const button = e.currentTarget as HTMLButtonElement;
        if (color === "orange") {
            button.classList.remove("lg:bg-orange-f3", "lg:border-orange-f3");
            button.classList.add("lg:bg-orange-f6");
        } else {
            button.classList.remove("lg:hover:bg-gray-6", "lg:border-gray-6");
            button.classList.add("lg:hover:bg-gray-4");
        }
    }
</script>

<style>
    .loader {
        border-style: solid;
        border-bottom-color: transparent;
        border-radius: 50%;
        display: inline-block;
        box-sizing: border-box;
        animation: rotation 1s linear infinite;
    }
    @keyframes rotation {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
    } 
</style>
