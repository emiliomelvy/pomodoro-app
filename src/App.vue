<script setup>
  import play from './assets/play.svg'
  import download from './assets/download.svg'
  import recycleBin from './assets/recyclebin.svg'
</script>

<template>
  <div class="flex justify-around">
    <div class="pt-10">
      <div class="w-96 h-96 bg-slate-100 shadow-2xl rounded-full flex justify-center items-center">
        <div class="w-80 h-80 bg-red-400 rounded-full flex justify-center items-center">
          <div class="w-64 h-64 bg-red-400 rounded-full flex justify-center border-[2px]">
            <h1 class="text-5xl text-center text-white my-auto">
              25:00
            </h1>
          </div>
        </div>
      </div>
      <div class="flex justify-between pt-16">
        <div class="text-5xl w-16 h-16 bg-slate-400 text-center rounded-full cursor-pointer">
          -
        </div>
        <div class="text-5xl w-16 h-16 bg-slate-400 text-center rounded-full cursor-pointer">
          +
        </div>
        <div class="text-5xl w-44 h-16 bg-slate-400 text-center rounded-full flex justify-center items-center cursor-pointer bg-lime-200">
          <img :src="play" alt="" class="w-8">
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
</template>