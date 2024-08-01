<script setup lang="ts">
import { ref } from 'vue';
import TheKlavier from './TheKlavier.vue';

interface Props {
    octaveNumber: number
}
defineProps<Props>()

const basicTones = ref(['C', 'D', 'E', 'F', 'G', 'A', 'B'])
const basicSemitones = ref(['', 'C#', 'D#', '', 'F#', 'G#', 'A#', ''])
</script>

<template>
    <div class="relative w-full h-full">
        <div class="flex flex-row items-center justify-center space-x-0.5 absolute top-0 w-full h-full">
            <template v-for="(key, idx) in basicTones" :key="idx">
                <TheKlavier :name="`${key}${octaveNumber}`" />
            </template>
        </div>

        <div class="flex flex-row space-x-0.5 absolute top-0 w-full h-[55%]">
            <template v-for="(key, idx) in basicSemitones" :key="idx">
                <div class="w-full h-full first:w-1/2 last:w-1/2">
                    <TheKlavier :name="`${key}${octaveNumber}`" :class="{ 'hidden': key === '' }" semitone />
                </div>
            </template>
        </div>
    </div>
</template>
