<script setup lang="ts">
import TheOctave from './TheOctave.vue';

</script>

<template>
  <div class="container flex flex-row w-full mx-auto h-full space-x-0.5">
    <TheOctave :number="4" />
    <TheOctave :number="5" />
  </div>
</template>
