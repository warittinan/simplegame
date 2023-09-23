<script setup>
import { ref, onBeforeMount } from 'vue'
const state = ref(0)
const countdowntime = ref(3)
const countdowntimer = ref(0)
const colorcode = ref(['text-blue-500', 'text-green-500', 'text-yellow-500', 'text-red-500', 'text-purple-500'])
const color = ref('')
const word = ref(['Blue', 'Green', 'Yellow', 'Red', 'Purple'])
const showword = ref('')
onBeforeMount(() => {
    state.value = 0
})
const next = () => {
    state.value++
}
const Level = (level) => {
    switch (level) {
        case 'Easy':
            countdowntimer.value = 30
            state.value++
            countdown()
            break;
        case 'Medium':
            countdowntimer.value = 20
            state.value++
            countdown()
            break;
        case 'Hard':
            countdowntimer.value = 10
            state.value++
            countdown()
            break;
    }
}
const countdown = () => {
    let timer = setInterval(() => {
        countdowntime.value--;
        if (countdowntime.value <= 0) {
            clearInterval(timer);
            state.value++
            randomword()
            randomcolor()
        }
    }, 1000);
}
const randomword = () => {
    let random = Math.floor(Math.random() * 5)
    showword.value = word.value[random]
}
const randomcolor = () => {
    let random = Math.floor(Math.random() * 5)
    word.value[random]
    if (word.value[random] == 'Blue') {
        color.value = colorcode.value[0]
    } else if (word.value[random] == 'Green') {
        color.value = colorcode.value[1]
    } else if (word.value[random] == 'Yellow') {
        color.value = colorcode.value[2]
    } else if (word.value[random] == 'Red') {
        color.value = colorcode.value[3]
    } else if (word.value[random] == 'Purple') {
        color.value = colorcode.value[4]
    }
}

const check = (word) => {
    if (color.value.includes(word.toLowerCase())) {
        randomword()
        randomcolor()
    }
}
const home = () => {
    state.value = 0
    countdowntime.value = 3
}
</script>
 
<template>
    <div class="w-screen h-screen  ">
        <div class="w-full h-full  flex flex-col justify-center space-y-5 items-center text-white " v-show="state == 0">
            <button class="w-32 h-16 bg-black/70 rounded-3xl text-2xl hover:bg-black " @click="next()">Play</button>
            <button class="w-32 h-16 bg-black/70 rounded-3xl text-2xl hover:bg-black  ">Settings</button>
        </div>
        <div class="w-full h-full  flex flex-col justify-center space-y-5 items-center text-white cursor-default  "
            v-show="state == 1">
            <p
                class="w-32 h-16 bg-blue-500 rounded-3xl text-2xl text-center flex items-center justify-center border-4 border-black">
                LEVEL</p>
            <button class="w-32 h-16 bg-green-500 rounded-3xl text-2xl hover:bg-green-600 border-4 border-black"
                @click="Level('Easy')">Easy</button>
            <button class="w-32 h-16 bg-yellow-500 rounded-3xl text-2xl hover:bg-yellow-600 border-4 border-black "
                @click="Level('Medium')">Medium</button>
            <button class="w-32 h-16 bg-red-500 rounded-3xl text-2xl hover:bg-red-600 border-4 border-black  "
                @click="Level('Hard')">Hard</button>
        </div>

        <div class="w-full h-full  flex justify-center  items-center text-black" v-show="state == 2">
            <h1>{{ countdowntime }}</h1>
        </div>
        <div class="w-full h-full  flex justify-center  items-center text-black " v-show="state == 3">
            <div class="w-[60%] h-[90%] bg-purple-200 flex flex-col items-center rounded-lg ">
                <div class="h-[5%] w-full bg-purple-900 rounded-t-lg flex justify-end  ">
                    <img src="./photo/remove.png" class="w-6 h-6 cursor-pointer hover:opacity-70 " @click="home">
                </div>
                <div class="h-[45%] w-[90%] bg-white flex flex-col justify-center items-center text-8xl mt-5 rounded-2xl ">
                    <h1 class=" font-bold " :class="color">{{ showword }}</h1>
                </div>
                <div class="w-full h-[30%] flex space-x-2 justify-center pt-5   ">
                    <div v-for="stringcolor in word">
                        <button class="w-20 h-16  rounded-3xl text-2xl text-white"
                            :class="stringcolor == 'Blue' ? 'bg-blue-500' : stringcolor == 'Yellow' ? 'bg-yellow-500' : stringcolor == 'Green' ? 'bg-green-500' : stringcolor == 'Red' ? 'bg-red-500' : 'bg-purple-500'"
                            @click="check(stringcolor)">{{ stringcolor }}</button>
                    </div>
                </div>

            </div>
        </div>
</div></template>
 
<style scoped></style>