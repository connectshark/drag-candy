<script setup>
import { ref } from 'vue'

const enter = ref(false)
const list = ref([])

const startDrag = (event, value) => {
  event.dataTransfer.dropEffect = 'move'
  event.dataTransfer.effectAllowed = 'move'
  event.dataTransfer.setData('value', value)
}
const stopDrag = (event) => {
  const value = event.dataTransfer.getData('value')
  list.value.push(value)
  enter.value = false
}
</script>

<template>
  <section class="min-h-screen grid grid-flow-row lg:grid-flow-col h-full">
    <aside
      class="flex items-center justify-center lg:flex-col lg:col-span-2 row-span-2 col-auto lg:row-auto"
    >
      <div
        class="w-24 bg-red-400 drop-shadow-xl rounded-xl text-2xl text-center mb-4"
        draggable="true"
        @dragstart="startDrag($event, 'bg-red-400')"
      >candy</div>
      <div
        class="w-24 bg-pink-400 drop-shadow-xl rounded-xl text-2xl text-center mb-4"
        draggable="true"
        @dragstart="startDrag($event, 'bg-pink-400')"
      >candy</div>
      <div
        class="w-24 bg-green-400 drop-shadow-xl rounded-xl text-2xl text-center mb-4"
        draggable="true"
        @dragstart="startDrag($event, 'bg-green-400')"
      >candy</div>
    </aside>
    <main
      class="lg:col-span-3 row-span-3 col-auto lg:row-auto"
      :class="{ 'bg-gray-100': enter }"
      @dragover.prevent="enter = true"
      @dragleave.prevent="enter = false"
      @drop="stopDrag($event)"
    >
      <TransitionGroup
        name="list"
        tag="ul"
        class="w-24 h-full mx-auto bg-gray-700 flex flex-col-reverse"
      >
        <div
          v-for="item in list"
          :key="item"
          class="h-1/6 rounded-xl text-2xl text-center"
          :class="item"
        >candy</div>
      </TransitionGroup>
    </main>
  </section>
</template>
<style>
.list-move,
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from {
  opacity: 0;
  transform: translateY(-500%);
}
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>