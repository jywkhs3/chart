<template>
  <div>
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<script setup>
import { Chart, registerables } from 'chart.js';
import { onMounted, ref } from 'vue';

const chartCanvas = ref(null);

Chart.register(...registerables);

  onMounted(()=>{
    new Chart (chartCanvas.value,{
      type: 'bar',
      data: {
        labels: ['Attendance','Late','Absence', 'Early Leave'],
        datasets: [
          {
            label: 'Dataset Example', 
            data: [10,5,12,2],
            backgroundColor: [
              'rgba(229,43,123,0.5)',
              'rgba(71,162,1,0.5)',
              'rgba(12,20,139,0.5)',
              'rgba(150,55,200,0.5)'
            ],

            borderColor: [
              'rgb(0,0,0)',
              'rgb(0,0,0)',
              'rgb(0,0,0)',
              'rgb(0,0,0)'
            ],
            borderWidth: 1
          }
        ]
      },
      options: {
      responsive: true,
      pugins: {
        title:{
          display: true,
          text: 'Chart.js View'
        }
      }
    }
    });

  });
</script>

<style lang="scss" scoped>

</style>