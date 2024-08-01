<script setup lang="ts">
import { reactive, watch } from 'vue';

interface Props {
    name: string,
    semitone?: boolean
}
defineProps<Props>()

const klavier = reactive({
    active: false,
    touchId: -1
})

function mouseover(e: MouseEvent) {
    if (e.buttons) {
        klavier.active = true
    }
}

function touchstart(e: TouchEvent) {
    e.preventDefault()
    klavier.touchId = e.touches[0].identifier
    klavier.active = true
}

watch(() => klavier.active, () => {
    if (!klavier.active) {
        klavier.touchId = -1
    }
})
</script>

<template>
    <button v-if="!semitone" @mouseover="mouseover" @mousedown="klavier.active = true" @touchstart="touchstart"
        @mouseleave="klavier.active = false" @mouseup="klavier.active = false" @touchend="klavier.active = false"
        class="select-none border-gray-200 border-2 shadow-[0_0.4rem_0rem_rgba(229,231,235,1)] rounded-xl bg-white transition-all duration-75 h-full w-full flex justify-center items-end"
        :class="{ '!shadow-none !border-sky-200 !bg-sky-50 translate-y-2': klavier.active }">
        <span class="mb-4">{{ name }}</span>
    </button>

    <button v-else @mouseover="mouseover" @mousedown="klavier.active = true" @touchstart="touchstart"
        @mouseleave="klavier.active = false" @mouseup="klavier.active = false" @touchend="klavier.active = false"
        class="select-none shadow-[0_0.4rem_0rem_rgba(41,37,36,1)] border-stone-800 text-stone-400 border-2 rounded-xl bg-stone-700 transition-all duration-75 h-full w-full flex justify-center items-end"
        :class="{ '!shadow-none !border-gray-800 !bg-gray-950 translate-y-2': klavier.active }">
        <span class="mb-4">{{ name }}</span>
    </button>
</template>
