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
        totalTime: (25*60),
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
        if(this.totalTime === 60) {
          return this.totalTime = 60 
        }
        else {
          this.totalTime -= 60
        }
      },
      padTime: function(time) {
        return (time < 10 ? '0' : '') + time
      },
      startTimer: function() {
        this.timer = setInterval(() => {
          this.totalTime--
        }, 1000)
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
      },
      resetTimer: function() {
        this.arr = [...this.arr, {
          hours: this.date.getHours(),
          mins: this.date.getMinutes(),
          second: this.date.getSeconds(),
          month: this.date.getMonth(),
          date: this.date.getDate(),
          day: this.date.getDay(),
          minutes1: (+this.copyMinutes - +this.minutes) <= 10 ? `0${(+this.copyMinutes - +this.minutes) - 1}` : `${(+this.copyMinutes - +this.minutes)}`,
          sec: +this.copySec - +this.seconds === 60 ? '00' : `${+this.copySec - +this.seconds < 10 ? '0' : ''}${+this.copySec - +this.seconds}`,
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
        localStorage.setItem('obj', JSON.stringify(this.arr))
      },
      deleteSess: function() {
        this.arr = []
        localStorage.clear('obj')
      },
      downloadObjectAsJson: function(){
        const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(window.localStorage.obj));
        const downloadAnchorNode = document.createElement('a');
        downloadAnchorNode.setAttribute("href", dataStr);
        downloadAnchorNode.setAttribute("download", 'logs' + ".json");
        document.body.appendChild(downloadAnchorNode); // required for firefox
        downloadAnchorNode.click();
        downloadAnchorNode.remove();
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
    updated() {
    if(this.minutes == 0) {
      this.resetTimer()
     } 
    }
  }

</script>

<template>
  <div v-if="showModal" class="container md:max-w-7xl 2xl:max-w-[999rem] mx-auto absolute overflow-hidden bg-black opacity-50 w-full h-[100rem]">
  </div>
  <div v-if="showModal" class="container md:max-w-7xl mx-auto absolute bg-sky-500 top-40 md:inset-x-96 inset-x-1 w-96 z-20 py-10">
    <h1 class="text-3xl text-center">
      Please add notes
    </h1>
    <div class="flex justify-center py-5">
      <input type="text border-8" placeholder="notes" v-model="this.activity" @keydown.enter="showModal = false">
    </div>
  </div>
  <div class="">

    <div v-if="!showModal" class="container mx-auto max-w-7xl">
      <p class="text-3xl p-3">
        You're doing {{this.activity}}
      </p>
    </div>
    <div class="container md:max-w-7xl px-5 md:mx-auto flex md:flex-row flex-col justify-around">
      <div class="py-10">
        <div class="w-80 h-80 bg-slate-100 shadow-2xl rounded-full flex justify-center mx-auto items-center">
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
          <div @click="downloadObjectAsJson()" class="flex basis-4/12 justify-center gap-2 bg-lime-200 rounded-full cursor-pointer">
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
            <td class="border pl-2 pr-10 py-5">{{ tes.hours }}:{{ tes.mins < 10 ? `0${tes.mins}` : tes.mins }}:{{ tes.second }}, {{ this.months[tes.month] }} {{ tes.date }}, {{ this.days[tes.day] }}</td>
            <td class="border pl-2 pr-10 py-5"><strong>{{ tes.minutes1 }} : {{ tes.sec }}</strong> / {{ tes.copyMinutes }} min.</td>
            <td class="border pl-2 pr-10 py-5">{{ tes.note }}</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>