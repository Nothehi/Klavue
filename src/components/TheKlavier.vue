<script setup lang="ts">
import { reactive, watch } from 'vue';

const props = defineProps<{
    id: number,
    name: string,
    semitone?: boolean
}>()

const klavier = reactive({
    active: false,
    touchId: -1
})

watch(() => klavier.active, () => {
    if (!klavier.active) {
        klavier.touchId = -1
    }
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

function setActiveState(state: boolean) {
    klavier.active = state
}

function setTouchId(id: number) {
    klavier.touchId = id
}

defineExpose({
    name: props.name,
    setActiveState,
    setTouchId
})
</script>

<template>
    <button @mouseover="mouseover" @mousedown="klavier.active = true" @touchstart="touchstart"
        @mouseleave="klavier.active = false" @mouseup="klavier.active = false" @touchend="klavier.active = false"
        class="flex items-end justify-center w-full h-full transition-all duration-75 border-2 select-none rounded-xl"
        :class="{
            'bg-white border-gray-200 shadow-[0_0.4rem_0rem_rgba(229,231,235,1)]': !semitone,
            '!shadow-none !border-sky-200 !bg-sky-50 translate-y-2': klavier.active && !semitone,
            'bg-stone-700 border-stone-800 shadow-[0_0.4rem_0rem_rgba(41,37,36,1)] text-stone-400': semitone,
            '!shadow-none !border-gray-800 !bg-gray-950 translate-y-2': klavier.active && semitone,
        }">
        <span class="mb-4 text-sm font-medium">{{ name }}</span>
    </button>
</template>
