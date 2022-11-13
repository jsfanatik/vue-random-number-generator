<template>
    <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
      <div class="px-4 py-6 sm:px-0">
        <div class="flex items-center justify-center py-16">
  
          <QuestionMarkCircleIcon v-if="slots.length < 1" class="w-96 h-96"/>
  
          <div v-else v-for="slot in slots" :key="slot.id" class="w-96 h-96 m-2">
            <div class="flex justify-center items-center w-full h-full p-8 text-9xl rounded-md shadow-lg border-solid border-2 border-sky-500">
              <!-- <img :src="slot.img"> -->
              {{ slot }}
            </div>
          </div>
        </div>
  
        <div class="flex justify-center">
          <button @click="spin" class="group relative w-48 flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
            Spin
          </button>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from "@vue/reactivity";
  import { onMounted } from "@vue/runtime-core";
  import { QuestionMarkCircleIcon } from "@heroicons/vue/outline"
  
  const slots = ref([])
    
  const numberArray = ref(Array.from({length: 9}, (e, i)=> i + 1))
  
  const sleep = (milliseconds) => {
    return new Promise(resolve => setTimeout(resolve, milliseconds))
  }
  
  let prevIndex = null
  const generateRandIndex = max => {
    let index
    do {
      // console.log('generating random number...')
      index = Math.floor(Math.random() * max)
    } while (index === prevIndex)
    // console.log(index)
    // console.log('p', prevIndex)
    prevIndex = index
    return index
  }
  
  const spin = async () => {
    const list = numberArray.value.sort(() => Math.random() - 0.5)
  
    for (let i = 0; i < 10; i++) {
      const index = generateRandIndex(list.length)
      await sleep(100)
      slots.value = list.filter(r => r === index + 1)
      console.log(slots.value)
    }
  }
  </script>