<template>
    <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
        <div class="px-4 py-6 sm:px-0">
            <span class="flex items-center justify-center mb-2 text-3xl font-bold">
                {{ score }} Points!
            </span>

            <span class="flex items-center justify-center text-md font-bold">
                <span>Match at least 1 value per spin to get 5 points!</span>
            </span>
            
            <div class="flex items-center justify-center py-16">
                <QuestionMarkCircleIcon v-if="slots.length < 1" class="w-96 h-96"/>
                <div v-else v-for="slot in slots" :key="slot.id" class="w-96 h-96 m-2">
                    <div class="flex justify-center items-center w-full h-full p-8 text-9xl rounded-md shadow-lg border-solid border-2 border-sky-500">
                        {{ slot }}
                    </div>
                </div>
            </div>

            <div class="flex items-center justify-center mb-12">
                <input v-model="inputValueOne" type="number" class="mt-4 m-2 border-2 border-solid border-sky-500" />
                <input v-model="inputValueTwo" type="number" class="mt-4 m-2 border-2 border-solid border-sky-500" />
                <input v-model="inputValueThree" type="number" class="mt-4 m-2 border-2 border-solid border-sky-500" />
            </div>
            
            <div class="flex justify-center">
                <button @click="spin" :disabled="disableButton" :class="[disableButton === true ? 'cursor-not-allowed' : 'cursor-pointer']" class="group relative w-48 flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600">
                Spin
                </button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from "@vue/reactivity";
import { onMounted } from "@vue/runtime-core";
import { QuestionMarkCircleIcon } from "@heroicons/vue/outline"

const inputValueOne = ref(null)
const inputValueTwo = ref(null)
const inputValueThree = ref(null)
const score = ref(0)
const slots = ref([])

const numberArray = ref(Array.from({length: 10}, (e, i)=> i))

const disableButton = computed(() => {
    if (inputValueOne.value === null 
    || inputValueTwo.value === null 
    || inputValueThree.value === null) {
        return true
    }
})

const sleep = (milliseconds) => {
    return new Promise(resolve => setTimeout(resolve, milliseconds))
}

const spin = async () => {
    for (let i = 0; i < numberArray.value.length; i++) {
        await sleep(200)
        slots.value = numberArray.value.sort((a, b) => 0.5 - Math.random()).slice(0, 3);
    }

    const guessArray = [inputValueOne.value, inputValueTwo.value, inputValueThree.value]

    const found = guessArray.some(r => [...slots.value].includes(r))

    if (found === true) {
        score.value += 5
    } else {
        score.value -= 5
    }
}  
</script>