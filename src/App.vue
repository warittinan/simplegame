<script setup>
import { ref, onBeforeMount } from 'vue'
const state = ref(0)
const countdowntime = ref(3)
const countdowntimer = ref(0)
const colorcode = ref(['text-blue-500', 'text-green-500', 'text-yellow-500', 'text-red-500', 'text-purple-500'])
const color = ref('')
const word = ref(['Blue', 'Green', 'Yellow', 'Red', 'Purple'])
const showword = ref('')
const size = ref(100)
const score = ref(0)
const calsize = ref(0)
onBeforeMount(() => {
    state.value = 0
    size.value = 100
})
const next = () => {
    state.value++
}
const Level = (level) => {
    switch (level) {
        case 'Easy':
            countdowntimer.value = 30
            state.value++
            calsize.value = 100 / 30

            countdown()
            break;
        case 'Medium':
            countdowntimer.value = 20
            state.value++
            calsize.value = 100 / 20
            countdown()
            break;
        case 'Hard':
            countdowntimer.value = 10
            state.value++
            calsize.value = 100 / 10
            countdown()
            break;
    }
}
const countdown = () => {
    let time = setInterval(() => {
        countdowntime.value--;
        if (countdowntime.value <= 0) {
            clearInterval(time);
            state.value++
            randomword()
            randomcolor()
            timer()
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

const check = (string) => {
    let number = Math.floor(Math.random() * 5)
   
    if (color.value.includes(string.toLowerCase())) {
        word.value[number] = word.value.splice(1, 1, word.value[number])[0];
        randomword()
        randomcolor()
        score.value++
    }
}
const home = () => {
    state.value = 0
    countdowntime.value = 3
    countdowntimer.value = 0
    score.value = 0
    size.value = 100
}
const timer = () => {
    let timer = setInterval(() => {
        countdowntimer.value--;
        size.value = size.value - calsize.value
        console.log(size.value);
        if (countdowntimer.value <= 0) {
            clearInterval(timer);
            state.value++
        }
    }, 1000);
}
</script>
 
<template>
    <div class="w-screen h-screen  bg-white cursor-default ">
        <div class="w-full h-full   text-white   " v-show="state == 0">
            <div class=" w-full h-[10%] bg-black justify-center items-center flex text-6xl">
                <h1>Click The Color Not The Word</h1>
            </div>
            <div class="w-full h-[90%] flex flex-col justify-center items-center space-y-5">
                <button class="w-32 h-16 bg-blue-500 rounded-3xl text-2xl hover:bg-blue-400 " @click="next()">Play</button>
                <button class="w-40 h-16 bg-blue-500 rounded-3xl text-2xl hover:bg-blue-400"
                    onclick="my_modal_2.showModal()">How to play</button>
                <dialog id="my_modal_2" class="modal">
                    <div class="modal-box">
                        <ul style="list-style-type:decimal">
                            <li>
                                เมื่อเกมเริ่มต้นตัวจับเวลาจะเริ่มนับถอยหลังจากค่าที่กำหนดตามระดับlevel
                            </li>
                            <li>
                                คลิกคำที่ตรงกับสีของข้อความไม่ใช่ตัวข้อความ ตัวอย่างเช่น หากคำว่า Red แสดงเป็นสีน้ำเงิน
                                ให้คลิกที่คำว่า Blue
                            </li>
                            <li>
                                ได้รับหนึ่งคะแนนจากการเลือกที่ถูกต้องแต่ละครั้ง
                            </li>
                            <li>เกมจะจบลงเมื่อเวลาถึง 0 </li>
                        </ul>

                    </div>
                    <form method="dialog" class="modal-backdrop">
                        <button>close</button>
                    </form>
                </dialog>
            </div>


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

        <div class="w-full h-full  flex justify-center  items-center text-blue-700 text-9xl font-bold" v-show="state == 2">
            <h1>{{ countdowntime }}</h1>
        </div>
        <div class="w-full h-full  flex justify-center  items-center text-black " v-show="state == 3">
            <div class="w-[60%] h-[90%] bg-blue-200 flex flex-col items-center rounded-lg  ">
                <div class="h-[5%] w-full bg-blue-900 rounded-t-lg flex justify-end  ">
                    <img src="./photo/remove.png" class="w-6 h-6 cursor-pointer hover:opacity-70 " @click="home">
                </div>
                <div class="w-full h-[2%] ">
                    <div class="w-full h-full bg-gray-200  dark:bg-gray-700">
                        <div :class="size < 30 ? 'bg-red-600 h-full' : 'bg-green-600 h-full'" :style="`width: ${size}%; `"
                            style="transition: width 1500ms linear 0s; "> </div>
                    </div>
                </div>
                <div class="h-[45%] w-[90%] bg-white flex flex-col justify-center items-center text-8xl mt-5  rounded-2xl ">
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
        <div class="w-full h-full flex justify-center items-center" v-show="state == 4">
            <div
                class="w-[40%] h-[50%] bg-blue-300 rounded-xl text-white flex  flex-col justify-center items-center cursor-default">
                <div class="w-full h-[20%] flex justify-center items-center">
                    <h1 class="text-5xl font-bold">Game over</h1>
                </div>
                <div class="w-full h-[20%] flex justify-center items-center">
                    <h1 class="text-5xl font-bold">Score: {{ score }}</h1>
                </div>
                <div class="w-full h-[20%] flex justify-center items-center">
                    <button class="w-32 h-16 rounded-3xl text-3xl bg-blue-900 hover:bg-blue-900/80 "
                        @click="home">Home</button>
                </div>
            </div>
        </div>
    </div>
</template>
 
<style scoped></style>