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
        changeTime: true,
        copyMinutes: null,
        copySec: 60,
        resetButton: false,
        userTime: 25,
        started: false,
        arr: [],
        date: '',
        days: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
        months: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],
        showModal: true,
        activity: '',
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
        if(this.changeTime === true) {
          this.copyMinutes = this.minutes
        }
        this.changeTime = false
      },
      pauseTimer: function() {
        clearInterval(this.timer)
        this.timer = null
        this.resetButton = true
        console.log('copyMinutes : ', this.copyMinutes)
        console.log('this.minutes : ', this.minutes)
      },
      resetTimer: function() {
        this.arr = [...this.arr, {
          hours: this.date.getHours(),
          mins: this.date.getMinutes(),
          second: this.date.getSeconds(),
          month: this.date.getMonth(),
          date: this.date.getDate(),
          day: this.date.getDay(),
          // minutes1: Math.floor(this.totalTime / 60) < 10 ? `0${Math.floor(this.totalTime / 60)}` : Math.floor(this.totalTime / 60), 
          minutes1: (+this.copyMinutes - +this.minutes) <= 1 ? `0${(+this.copyMinutes - +this.minutes) - 1}` : `0${(+this.copyMinutes - +this.minutes)}`,
          // sec: this.totalTime % 60 < 10 ? `0${this.totalTime % 60}` : this.totalTime % 60,
          sec: +this.copySec - +this.seconds === 60 ? '00' : `${+this.copySec - +this.seconds}`,
          note: this.activity,
          copyMinutes: this.copyMinutes
        },
        ]
        clearInterval(this.timer)
        this.timer = null
        this.totalTime = (25*60)
        this.resetButton = false
        this.started = false
        this.activity = ''
        this.showModal = true
        this.changeTime = true
      },
      deleteSess: function() {
        this.arr = []
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
  <div v-if="showModal" class="container max-w-7xl mx-auto absolute overflow-hidden bg-black opacity-50 w-full h-[9999px]">
  </div>
  <div v-if="showModal" class="container max-w-7xl mx-auto absolute bg-sky-500 top-40 right-1/2 w-96 z-20 py-10">
    <h1 class="text-3xl text-center">
      Please add notes
    </h1>
    <div class="flex justify-center py-5">
      <input type="text border-8" placeholder="notes" v-model="this.activity" @keydown.enter="showModal = false">
    </div>
  </div>
  <div class="">

    <div v-if="!showModal" class="container mx-auto max-w-7xl">
      <p>
        You're doing {{this.activity}}
      </p>
    </div>
    <div class="container max-w-7xl mx-auto flex justify-around overflow-hidden">
      <div class="pt-10">
        <div class="w-80 h-80 bg-slate-100 shadow-2xl rounded-full flex justify-center items-center">
          <div class="w-64 h-64 bg-red-400 rounded-full flex justify-center items-center">
            <div class="w-52 h-52 bg-red-400 rounded-full flex justify-center border-[2px]">
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
        <div class="flex justify-between pt-16 gap-5">
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
        <div class="flex justify-end gap-3 py-5">
          <div class="flex basis-4/12 justify-center gap-2 bg-lime-200 rounded-full cursor-pointer">
            <img :src="download" alt="" class="w-4 py-2 my-auto">
            <p class="text-xs py-2 my-auto">
              Download logs
            </p>
          </div>
          <div class="flex cursor-pointer" @click="deleteSess">
            <img :src="recycleBin" alt="" class="w-4 py-2 my-auto">
            <p class="text-xs py-2 my-auto">
              Delete all sessions
            </p>
          </div>
        </div>
        <table class="table-auto">
          <tr class="border rounded-full">
            <th class="border px-5 py-3">#</th>
            <th class="border pl-2 pr-10 py-3">Started at</th>
            <th class="border pl-2 pr-10 py-3">Duration</th>
            <th class="border pl-2 pr-10 py-3">Notes</th>
          </tr>
          <tr class="border" v-for="(tes, key) in this.arr" :key="key">
            <td class="border pl-2 pr-10 py-5">{{ key + 1 }}</td>
            <td class="border pl-2 pr-10 py-5">{{ tes.hours }}:{{ tes.mins }}:{{ tes.second }}, {{ this.months[tes.month] }} {{ tes.date }}, {{ this.days[tes.day] }}</td>
            <td class="border pl-2 pr-10 py-5"><strong>{{ tes.minutes1 }} : {{ tes.sec }}</strong> / {{ tes.copyMinutes }} min.</td>
            <td class="border pl-2 pr-10 py-5">{{ tes.note }}</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>