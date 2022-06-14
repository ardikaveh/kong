<template>
  <KModalFullscreen
    :is-visible="props.serviceModalIsOpen"
    title="Add Service"
    @canceled="modalToggle"
    @proceed="modalToggle"
  />
</template>
<script lang="ts" setup>
import { defineProps, defineEmits } from 'vue'
import { KModalFullscreen } from '@kong/kongponents'
const props = defineProps<{
  serviceModalIsOpen: boolean
}>()
const emit = defineEmits<{(event: 'toggleModal'): void}>()
function modalToggle() {
  emit('toggleModal')
}
</script>
<style>
@import "../../node_modules/@kong/kongponents/dist/style.css";
</style>
