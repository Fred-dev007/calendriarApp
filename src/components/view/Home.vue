<script setup>
import { ref } from 'vue'
let days = ref([
  { name: 'Sunday', tache: ['Prendre un Café', 'Manger', 'Prendre du pain'] },
  { name: 'Monday', tache: [] },
  { name: 'Tuesday', tache: [] },
  { name: 'Wesdnesday', tache: [] },
  { name: 'Thursday', tache: [] },
  { name: 'Friday', tache: [] },
  { name: 'Saturday', tache: ['Faire un paris'] },
])

let checkDay = ref('Sunday')
let event = ref()
let modif = ref()
let day = ref()
let ev = ref()
let index = ref()

function send(t) {
  checkDay.value = t.name
  day.value = t
  modif.value = ''
  event.value = ''
}

function addEvent(d) {
  if (day.value) {
    if (event.value) {
      d.tache.push(event.value)
      event.value = ''
    }
  }else{
    if (event.value) {
      days.value[0].tache.push(event.value)
      event.value = ''
    }
  }
}

function deleteEvt(tab, evt) {
  Swal.fire({
    title: 'Are you sure?',
    text: "You won't be able to revert this!",
    icon: 'warning',
    showCancelButton: true,
    confirmButtonColor: '#3085d6',
    cancelButtonColor: '#d33',
    confirmButtonText: 'Yes, delete it!',
  }).then(result => {
    if (result.isConfirmed) {
      tab.tache = tab.tache.filter(t => t != evt)
      Swal.fire({
        title: 'Deleted!',
        text: 'Your event has been deleted.',
        icon: 'success',
      })
    }
  })
}

function update(t, i) {
  ev.value = t
  index.value = i
  modif.value = t.tache[i]
  event.value = modif.value
}
function submitEvt() {
  ev.value.tache[index.value] = event.value
  event.value = ''
  modif.value = ''
  Swal.fire({
  position: "top-end",
  icon: "success",
  title: "Evenement modifié avec success",
  showConfirmButton: false,
  timer: 2000
});
}



</script>

<template>
  <body class="bg-gray-200">
    <div class="container mx-auto pt-60 pb-20">
      <h1 class="text-center absolute top-10 left font-bold text-5xl  font-mono">Calendar App</h1>
      <div class="bg-white p-10 md:w-2/3 lg:w-1/2 mx-auto rounded">
        <form action="" @submit.prevent="!modif ? addEvent(day) : submitEvt()">
          <div class="flex items-center mb-5">
            <label
              for="name"
              class="w-20 inline-block text-right mr-4 text-gray-500 text-gray-500"
              >Evenement:</label
            >
            <input
              v-model="event"
              name="name"
              id="name"
              type="text"
              placeholder="Your event"
              class="border-b-2 border-gray-400 flex-1 py-2 placeholder-gray-300 outline-none focus:border-green-400"
            />
          </div>
          <div class="text-gray-300">Day of the event: {{ checkDay }}</div>
          <div class="text-right">
            <button
              v-if="modif"
              class="py-3 px-8 bg-green-500 text-green-100 font-bold rounded"
            >
              Modifier
            </button>
            <button
              v-else
              class="py-3 px-8 bg-green-500 text-green-100 font-bold rounded"
            >
              Ajouter
            </button>
          </div>
        </form>
      </div>

      <div class="wrapper bg-white rounded shadow w-full mt-20 mb-80">
        <table class="w-full">
          <thead>
            <tr>
              <th
                v-for="(day, index) in days"
                :key="index"
                class="p-2 border-r h-10 xl:w-40 lg:w-30 md:w-30 sm:w-20 w-10 xl:text-sm text-xl"
              >
                <span class="xl:block lg:block md:block sm:block">{{
                  day.name
                }}</span>
              </th>
            </tr>
          </thead>
          <tbody>
            <tr class="text-center h-full z-0">
              <td
                v-for="(day, index) in days"
                :key="index"
                @click="send(day)"
                class="border p-1 xl:w-40 lg:w-30 md:w-30 sm:w-20 w-10 overflow-auto transition cursor-pointer duration-500 ease hover:bg-gray-50"
              >
                <div
                  class="flex flex-col  xl:w-40 lg:w-30 md:w-30 sm:w-full w-10 mx-auto"
                >
                  <div class="bottom flex-grow h-30 py-1 w-full cursor-pointer">
                    <div
                      v-for="(tache, index) in day.tache"
                      :key="index"
                      class="event bg-blue-500 text-white rounded p-3 text-sm mb-2 hover:skew-x-1 hover:scale-110 transition-all duration-500 break-all" 
                      @dblclick="update(day, index)"
                    >
                      <img
                        src="../square-xmark-solid.svg"
                        class="del w-5 hidden transition-display duration-600 ease"
                        alt=""
                        @click="deleteEvt(day, tache)"
                      />
                      <span class="event-name">
                        {{ tache }}
                      </span>
                    </div>
                  </div>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </body>
</template>

<style>
.event:hover > .del {
  display: block;
  position: absolute;
  left: 0;
  top: 0;
}
body {
  background: url(../top-view-planner-calendar-with-leaves.jpg) no-repeat;
  background-size: cover;
}
</style>
