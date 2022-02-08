<script setup>
import { ref } from 'vue'

const enter = ref(false)
const err = ref(false)
const list = ref([])

const startDrag = (event, value) => {
  event.dataTransfer.dropEffect = 'move'
  event.dataTransfer.effectAllowed = 'move'
  event.dataTransfer.setData('value', value)
}
const detectAmount = () => {
  if (list.value.length >= 6) {
    err.value = true
  } else {
    enter.value = true
  }
}
const detectAmountLeave = () => {
  err.value = false
  enter.value = false
}
const stopDrag = (event) => {
  if (list.value.length >= 6) {
    err.value = false
    return
  }
  const value = event.dataTransfer.getData('value')
  list.value.push({ color: value, key: new Date().getTime() })
  enter.value = false
}
const deleteItem = key => {
  const i = list.value.findIndex(item => item.key === key)
  list.value.splice(i, 1)
}
</script>

<template>
  <section class="min-h-screen grid grid-flow-row lg:grid-flow-col h-full grid-rows-5 lg:grid-rows-none grid-cols-none lg:grid-cols-5">
    <aside
      class="flex items-center justify-center lg:flex-col lg:col-span-2 row-span-1 col-auto lg:row-auto"
    >
      <div
        class="w-24 bg-red-400 drop-shadow-xl rounded-xl text-2xl text-center m-4"
        draggable="true"
        @dragstart="startDrag($event, 'bg-red-400')"
      >candy</div>
      <div
        class="w-24 bg-pink-400 drop-shadow-xl rounded-xl text-2xl text-center m-4"
        draggable="true"
        @dragstart="startDrag($event, 'bg-pink-400')"
      >candy</div>
      <div
        class="w-24 bg-green-400 drop-shadow-xl rounded-xl text-2xl text-center m-4"
        draggable="true"
        @dragstart="startDrag($event, 'bg-green-400')"
      >candy</div>
    </aside>
    <main
      class="lg:col-span-3 row-span-4 col-auto lg:row-auto"
      :class="{ 'bg-gray-100': enter, 'bg-red-200': err }"
      @dragover.prevent="detectAmount"
      @dragleave.prevent="detectAmountLeave"
      @drop="stopDrag($event)"
    >
      <TransitionGroup
        name="list"
        tag="ul"
        class="w-24 h-full mx-auto bg-gray-700 flex flex-col-reverse"
      >
        <div
          v-for="item in list"
          :key="item.key"
          class="h-1/6 rounded-xl text-2xl text-center"
          :class="item.color"
        ><div @click="deleteItem(item.key)">X</div></div>
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