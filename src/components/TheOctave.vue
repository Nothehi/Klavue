<script setup lang="ts">
import { computed, ref } from 'vue';
import TheKlavier from './TheKlavier.vue';

interface Props {
    number: number
    start?: string,
    end?: string
}
const props = defineProps<Props>()

const klavierRefs = ref<Array<InstanceType<typeof TheKlavier> | null | unknown>>([]);
const basicOctave = ref(['c', 'c#', 'd', 'd#', 'e', '', 'f', 'f#', 'g', 'g#', 'a', 'a#', 'b'].map((note) => note ? `${note}${props.number}` : ''))
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

function touchmove(e: TouchEvent) {
    Array.from(e.changedTouches).forEach((touch) => {
        const noteName = document.elementFromPoint(touch.clientX, touch.clientY)?.textContent
        if (!noteName) {
            klavierRefs.value.forEach((ref: any) => ref.setActiveState(false))
            return
        }

        const klavier: any = klavierRefs.value.find((ref: any) => ref.name.includes(noteName))
        if (klavier) {
            klavierRefs.value.forEach((ref: any) => ref.setActiveState(false))
            klavier.setActiveState(true)
            klavier.setTouchId(touch.identifier)
        }
    })
}

function touchend() {
    klavierRefs.value.forEach((ref: any) => ref.setActiveState(false))
}
</script>

<template>
    <div class="relative w-full h-full" @touchmove="touchmove" @touchend="touchend">
        <div class="flex flex-row items-center justify-center space-x-0.5 absolute w-full h-full">
            <template v-for="(note, idx) in octave" :key="idx">
                <TheKlavier v-if="!isSemitone(note)" :ref="(el) => klavierRefs.push(el)" :id="idx + (number - 1) * 12"
                    :name="note.toUpperCase()" />
            </template>
        </div>

        <div class="flex flex-row space-x-0.5 absolute -top-1 w-full h-[55%]">
            <div class="w-1/2 h-full"></div>

            <template v-for="(note, idx) in octave" :key="idx">
                <div v-if="isSemitone(note)" class="w-full h-full">
                    <TheKlavier v-if="note !== ''" :ref="(el) => klavierRefs.push(el)" :id="idx + (number - 1) * 12"
                        :name="note.toUpperCase().replace('#', '♯')" semitone />
                </div>
            </template>

            <div class="w-1/2 h-full"></div>
        </div>
    </div>
</template>
