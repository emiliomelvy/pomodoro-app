<script>
  import play from './assets/play.svg'
  import download from './assets/download.svg'
  import recycleBin from './assets/recyclebin.svg'
  import pause from './assets/pause.svg'
  import reset from './assets/reset.svg'

  export default {
    data() {
      return {
        play,
        download,
        recycleBin,
        pause,
        reset,
        timer: null,
        totalTime: (5*60),
        resetButton: false,
        userTime: 25,
        started: false,
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
      },
      pauseTimer: function() {
        clearInterval(this.timer)
        this.timer = null
        this.resetButton = true
      },
      resetTimer: function() {
        clearInterval(this.timer)
        this.timer = null
        this.totalTime = (25*60)
        this.resetButton = false
        this.started = false
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
            <img :src="reset" alt="" class="w-8">
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
          <tr class="border">
            <td class="border">1</td>
            <td class="border">16:27:45, Sep 1, Thu</td>
            <td class="border"><strong>0:07</strong> / 25 min.</td>
            <td class="border">  </td>
          </tr>
          <tr class="border">
            <td class="border">2</td>
            <td class="border">16:28:48, Sep 1, Thu</td>
            <td class="border"><strong>00:16</strong> / 25 min.</td>
            <td class="border">makan nasi ayam</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>