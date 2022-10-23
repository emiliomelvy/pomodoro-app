<script>
  import play from './assets/play.svg'
  import download from './assets/download.svg'
  import recycleBin from './assets/recyclebin.svg'
  import pause from './assets/pause.svg'
  import reset from './assets/reset.svg'
  import stop from './assets/stop.svg'

  export default {
    data() {
      return {
        play,
        download,
        recycleBin,
        pause,
        reset,
        stop,
        timer: null,
        totalTime: (5*60),
        copyTime: null,
        resetButton: false,
        userTime: 25,
        started: false,
        arr: [],
        date: '',
        days: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
        months: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],
        showModal: true,
        testing: '',
      }
    },
    methods: {
      add: function() {
        this.totalTime += 60
      },
      min: function() {
        this.totalTime -= 60
      },
      padTime: function(time) {
        return (time < 10 ? '0' : '') + time
      },
      startTimer: function() {
        this.timer = setInterval(() => {
          this.totalTime--
        }, 1)
        this.started = true
        this.date = new Date()
      },
      pauseTimer: function() {
        clearInterval(this.timer)
        this.timer = null
        this.resetButton = true
        console.log('copy : ', this.copyTime)
        console.log(Math.floor(this.copyTime / 60) - this.minutes)
      },
      resetTimer: function() {
        this.arr = [...this.arr, {
          hours: this.date.getHours(),
          mins: this.date.getMinutes(),
          second: this.date.getSeconds(),
          month: this.date.getMonth(),
          date: this.date.getDate(),
          day: this.date.getDay(),
          minutes1: Math.floor(this.totalTime / 60) < 10 ? `0${Math.floor(this.totalTime / 60)}` : Math.floor(this.totalTime / 60), 
          // minutes1: Math.floor((this.totalTime - +this.minutes) / 60),
          sec: this.totalTime % 60 < 10 ? `0${this.totalTime % 60}` : this.totalTime % 60,
          // sec: this.totalTime - +this.seconds,
          note: this.testing
        },
        ]
        clearInterval(this.timer)
        this.timer = null
        this.totalTime = (25*60)
        this.resetButton = false
        this.started = false
        this.testing = ''
        this.showModal = true
      },
      enterinput: function(val) {
        // this.testing += 
        console.log(val)
      },
      get: function(val) {
        console.log(val
        )
      }
    },
    computed: {
      minutes: function(){
        const minutes = Math.floor(this.totalTime / 60)
        if(minutes <= 0) {
          clearInterval(this.timer)
          this.timer = null
          this.resetButton = true
        }
        return this.padTime(minutes)
      },
      seconds: function() {
        const seconds = this.totalTime - (this.minutes * 60)
        if(this.minutes === '00') {
          this.timer = null
          this.resetButton = true
          return '00'
        }
        return this.padTime(seconds);
      }
    },
    mounted() {
      this.copyTime = this.totalTime
    }
  }

</script>

<template>
  <!-- <div class="relative top-20 mx-auto p-5 border w-96 shadow-lg rounded-md bg-white">
    <div class="mt-3 text-center">
      <div class="mx-auto flex items-center justify-center h-12 w-12 rounded-full bg-red-100">
        <svg class="h-6 w-6 text-red-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"
        xmlnx="http://www.w.org/2000/svg">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" 
          d="M5 13l4 4L19 7">
          </path>
        </svg>
      </div>
      <h3 class="text-lg leading-6 font-medium text-gray-900">Successfull</h3>
      <div class="mt-2 px-7 py-3">
        <p class="text-sm text-gray-500">Account has been Successfull registered.</p>
      </div>
      <div class="items-center px-4 py-3">
        <button id="ok-btn"
          class="px-4 py-2 bg-red-500 text-white 
          text-base font-medium rounded-md w-full 
          shadow-sm hover:bg-red-600 focus:outline-none focus:ring-2 focus:ring-purple-300">
          OK
        </button>
      </div>
    </div>
  </div> -->
  <div v-if="showModal" class="absolute shadow-[0 0 0 1000rem rgba(0, 0, 0, 0.5)] w-full h-[100vh]">
    <div class="relative bg-sky-500 top-40 w-96 mx-auto z-20">
      <h1 class="text-3xl text-center">
        Please add notes
      </h1>
      <input type="text border-8" placeholder="notes" v-model="this.testing" @keydown.enter="showModal = false">
      <button @click="showmodal = false" class="bg-red-500">enter</button>
    </div>
  </div>
  <div class="container max-w-7xl">
    <div class="flex justify-around">
      <div class="pt-10">
        <div class="w-96 h-96 bg-slate-100 shadow-2xl rounded-full flex justify-center items-center">
          <div class="w-80 h-80 bg-red-400 rounded-full flex justify-center items-center">
            <div class="w-64 h-64 bg-red-400 rounded-full flex justify-center border-[2px]">
              <h1 class="text-5xl text-center text-white my-auto">
                <span>
                  {{ minutes }}
                </span>
                :
                <span>
                  {{ seconds }}
                </span>
              </h1>
            </div>
          </div>
        </div>
        <div class="flex justify-between pt-16">
          <div @click="min" v-if="!started" class="text-5xl w-16 h-16 bg-slate-400 text-center rounded-full cursor-pointer">
            -
          </div>
          <div @click="add" v-if="!started" class="text-5xl w-16 h-16 bg-slate-400 text-center rounded-full cursor-pointer">
            +
          </div>
          <div @click="startTimer" v-if="timer === null" class="text-5xl w-44 h-16 bg-slate-400 text-center rounded-full flex justify-center items-center cursor-pointer bg-lime-200" :class="this.minutes === '00' ? 'hidden' : ''">
            <img :src="play" alt="" class="w-8">
          </div>
          <div @click="pauseTimer" v-if="timer" class="text-5xl w-44 h-16 bg-slate-400 text-center rounded-full flex justify-center items-center cursor-pointer bg-lime-200">
            <img :src="pause" alt="" class="w-8">
          </div>
          <div @click="resetTimer" v-if="resetButton" class="text-5xl w-44 h-16 bg-slate-400 text-center rounded-full flex justify-center items-center cursor-pointer bg-lime-200">
            <img :src="stop" alt="" class="w-8">
          </div>
        </div>
      </div>
      <div class="pt-20">
        <div class="flex justify-end gap-3">
          <div class="flex basis-4/12 justify-center gap-2 bg-lime-200 rounded-full cursor-pointer">
            <img :src="download" alt="" class="w-4 py-2 my-auto">
            <p class="text-xs py-2 my-auto">
              Download logs
            </p>
          </div>
          <div class="flex cursor-pointer">
            <img :src="recycleBin" alt="" class="w-4 py-2 my-auto">
            <p class="text-xs py-2 my-auto">
              Delete all sessions
            </p>
          </div>
        </div>
        <table class="table-auto">
          <tr class="border rounded-full">
            <th class="border">#</th>
            <th class="border">Started at</th>
            <th class="border">Duration</th>
            <th class="border">Notes</th>
          </tr>
          <tr class="border" v-for="(tes, key) in this.arr" :key="key">
            <td class="border">{{ key + 1 }}</td>
            <td class="border">{{ tes.hours }}:{{ tes.mins }}:{{ tes.second }}, {{ this.months[tes.month] }} {{ tes.date }}, {{ this.days[tes.day] }}</td>
            <td class="border"><strong>{{ tes.minutes1 }} : {{ tes.sec }}</strong> / 25 min.</td>
            <td class="border">{{ tes.note }}</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>