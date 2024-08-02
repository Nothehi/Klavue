<script setup lang="ts">
import { computed, ref } from 'vue';
import TheKlavier from './TheKlavier.vue';

interface Props {
    number: number
    start?: string,
    end?: string
}
const props = defineProps<Props>()

const basicOctave = ref(['c', 'c#', 'd', 'd#', 'e', '', 'f', 'f#', 'g', 'g#', 'a', 'a#', 'b'])
const octave = computed(() => {
    if (!props.start && !props.end) {
        return basicOctave.value
    }

    const startIdx = basicOctave.value.findIndex((val) => val == props.start?.toLowerCase().replace('#', ''))
    const endIdx = basicOctave.value.findIndex((val) => val == props.end?.toLowerCase().replace('#', ''))

    return basicOctave.value.slice(startIdx, endIdx + 1)
})

function isSemitone(note: string): boolean {
    return note.includes('#') || note === ''
}
</script>

<template>
    <div class="relative w-full h-full">
        <div class="flex flex-row items-center justify-center space-x-0.5 absolute w-full h-full">
            <template v-for="(note, idx) in octave" :key="idx">
                <TheKlavier v-if="!isSemitone(note)" :name="`${note.toUpperCase()}${number}`" />
            </template>
        </div>

        <div class="flex flex-row space-x-0.5 absolute -top-1 w-full h-[55%]">
            <div class="w-1/2 h-full"></div>

            <template v-for="(note, idx) in octave" :key="idx">
                <div v-if="isSemitone(note)" class="w-full h-full">
                    <TheKlavier v-if="note !== ''" :name="`${note.toUpperCase().replace('#', '♯')}${number}`"
                        semitone />
                </div>
            </template>

            <div class="w-1/2 h-full"></div>
        </div>
    </div>
</template>
