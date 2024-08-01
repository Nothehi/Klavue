<script setup lang="ts">
import { computed, ref } from 'vue';
import TheKlavier from './TheKlavier.vue';

interface Props {
    octaveNumber: number
    start?: string,
    end?: string
}
const props = defineProps<Props>()

const basicTones = ref(['c', 'd', 'e', 'f', 'g', 'a', 'b'])
const basicSemitones = ref(['', 'c#', 'd#', '', 'f#', 'g#', 'a#', ''])

const tones = computed(() => {
    if (!props.start && !props.end) {
        return basicTones.value
    }

    const startIdx = basicTones.value.findIndex((val) => val == props.start?.toLowerCase())
    const endIdx = basicTones.value.findIndex((val) => val == props.end?.toLowerCase())

    return basicTones.value.slice(startIdx, endIdx + 1)
})

const semitones = computed(() => {
    if (!props.start && !props.end) {
        return basicSemitones.value
    }

    const startIdx = basicTones.value.findIndex((val) => val == props.start?.toLowerCase())
    const endIdx = basicTones.value.findIndex((val) => val == props.end?.toLowerCase())

    return basicSemitones.value.slice(startIdx, endIdx + 2)
})
</script>

<template>
    <div class="relative w-full h-full">
        <div class="flex flex-row items-center justify-center space-x-0.5 absolute top-0 w-full h-full">
            <template v-for="(key, idx) in tones" :key="idx">
                <TheKlavier :name="`${key.toUpperCase()}${octaveNumber}`" />
            </template>
        </div>

        <div class="flex flex-row space-x-0.5 absolute top-0 w-full h-[55%]">
            <template v-for="(key, idx) in semitones" :key="idx">
                <div class="w-full h-full first:w-1/2 last:w-1/2 first:invisible last:invisible">
                    <TheKlavier :name="`${key.toUpperCase().replace('#', '♯')}${octaveNumber}`"
                        :class="{ 'invisible': key === '' }" semitone />
                </div>
            </template>
        </div>
    </div>
</template>
