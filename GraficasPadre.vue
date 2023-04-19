<script lang="ts" setup>
import axios from 'axios'
import { onMounted, onUnmounted, reactive } from 'vue'
import ScrolSync from '@/components/ScrollBar.vue'

import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
} from 'chart.js'
import { Bar } from 'vue-chartjs'

ChartJS.register(CategoryScale, LinearScale, BarElement, Title, Tooltip, Legend)
/*
async function pedirGraficas() {
  const response = await axios.get<any>('')
}
*/

onMounted(() => {
  console.log('hola')
})


const cpu = {
  labels: [
    'Enero',
    'Febrero',
    'Marzo',
    'Abril',
    'Mayo',
    'Junio',
    'Julio',
    'Augosto',
    'Septiembre',
    'Octubre',
    'Noviembre',
    'Deciembre'
  ],
  datasets: [
    {
      label: 'Consumo',
      backgroundColor: '#f87979',
      barThickness: 40,
      borderRadius: 5,
      data: [40, 20, 12, 80, 50, 40, 39, 40, 40, 20, 12, 11]
    }
  ]
}

const io = {
  labels: [
    'Enero',
    'Febrero',
    'Marzo',
    'Abril',
    'Mayo',
    'Junio',
    'Julio',
    'Augosto',
    'Septiembre',
    'Octubre',
    'Noviembre',
    'Deciembre'
  ],
  datasets: [
    {
      label: 'Consumo',
      backgroundColor: '#f87979',
      barThickness: 40,
      borderRadius: 5,
      data: [5, 27, 63, 32, 71, 40, 39, 40, 2, 19, 12, 38]
    }
  ]
}
const sec = {
  labels: [
    'Enero',
    'Febrero',
    'Marzo',
    'Abril',
    'Mayo',
    'Junio',
    'Julio',
    'Augosto',
    'Septiembre',
    'Octubre',
    'Noviembre',
    'Deciembre'
  ],
  datasets: [
    {
      label: 'Consumo',
      backgroundColor: '#f87979',
      barThickness: 40,
      borderRadius: 5,
      data: [8, 14, 12, 80, 50, 90, 9, 15, 30, 20, 12, 11]
    }
  ]
}

let charts: {
  labels: string[]
  datasets:
    | {
        label: string
        backgroundColor: string
        barThickness: number
        borderRadius: number
        data: number[]
      }[]
    | {
        label: string
        backgroundColor: string
        barThickness: number
        borderRadius: number
        data: number[]
      }[]
    | {
        label: string
        backgroundColor: string
        barThickness: number
        borderRadius: number
        data: number[]
      }[]
}[] = []

const options = {
  responsive: true,
  maintainAspectRatio: true
}

onMounted(async () => {
  //await pedirGraficas()
  //window.addEventListener('scroll', handleScroll)
  charts.push(cpu)
  charts.push(io)
  charts.push(sec)
})

let graficas: Element[] = reactive([])

onUnmounted(() => {
  //window.removeEventListener('scroll', handleScroll)
})

// function handleScroll(event: any) {
//   let position: number = event._vts
//   let scrollOptions = {
//     left: position,
//     top: 0,
//     behavior: undefined
//   }
//   if (event._vts) {
//     if (graficas.length !== 0) {
//       console.log('muevo el array')
//       console.log('asi tengo el array -> ' + graficas)
//       const el0 = document.getElementsByClassName('container').item(0)
//       el0?.scroll(scrollOptions)
//       const el1 = document.getElementsByClassName('container').item(1)
//       el1?.scroll(scrollOptions)
//       const el2 = document.getElementsByClassName('container').item(2)
//       el2?.scroll(scrollOptions)
//     } else {
//       console.log('lleno el array')
//       for (let index = 0; index < document.getElementsByClassName('container').length; index++) {
//         graficas.push(document.getElementsByClassName('container')[index])
//         document.getElementsByClassName('container')[index].scrollBy(event._vts, 0)
//         console.log(document.getElementsByClassName('container')[index])
//       }
//       graficas.shift()
//       console.log('asi queda el array -> ' + graficas)
//     }
//     //const el = document.getElementsByClassName('container').item(1)?.scrollTo()
//    }
// }
</script>
<template>
  <v-container>
    <v-row class="text-center">
      <!--
        
      -->
      <v-col class="text-md-center" cols="4">
        <h2>CPU</h2>
        <ScrolSync>
          <Bar :data="cpu" :options="options" />
        </ScrolSync>
      </v-col>
      <v-col class="text-md-center" cols="4">
        <h2>IO</h2>
        <ScrolSync>
          <Bar :data="io" :options="options" />
        </ScrolSync>
      </v-col>
      <v-col class="text-md-center" cols="4">
        <h2>SEC</h2>
        <ScrolSync>
          <Bar :data="sec" :options="options" />
        </ScrolSync>
      </v-col>

      <!--
          
        <v-col class="text-md-center" cols="4">
          <div class="container">
            <div class="containerBody">
              <h2>CPU</h2>
              <Bar :data="cpu" :options="options" />
            </div>
          </div>
        </v-col>
        <v-col class="text-md-center" cols="4">
          <div class="container">
            <div class="containerBody">
              <h2>IO</h2>
              <Bar :data="io" :options="options" />
            </div>
          </div>
        </v-col>
        <v-col class="text-md-center" cols="4">
          <div class="container">
            <div class="containerBody">
              <h2>SEC</h2>
              <Bar :data="sec" :options="options" />
            </div>
          </div>
        </v-col>
      -->
    </v-row>
  </v-container>
</template>
<style scoped>
h2 {
  color: white;
}
.container {
  margin-top: 1%;
  margin-bottom: 1%;
  width: 100%;
  overflow: auto;
}
.containerBody {
  width: 120%;
}
::-webkit-scrollbar {
  width: 10px;
  height: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  background: var(--lightestgrey);
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: white;
  border-radius: 5px;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: white;
}
</style>
