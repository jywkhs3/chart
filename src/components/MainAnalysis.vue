<template>
  <div class="main-analysis">
    <h2>"Overall Attendance Status"</h2>
    <div class="chart-container">
      <canvas ref="chartCanvas"></canvas>
    </div>
  </div>
</template>

<script setup>
import { Chart, registerables } from 'chart.js';
import { ref, watch, nextTick } from 'vue';

  const props= defineProps({
    students: Array
  });
  const chartCanvas = ref(null);
  let chartInst = null;
  Chart.register(...registerables);

  //전체 출력 데이터
  const attendanceCount = {present:0, leave:0, absent:0, late:0};

  //학생 전체 출결 데이터 카운트
  watch(()=>props.students,async()=>{
    props.students.forEach((list)=>{
    // console.log(list);
    list.attendance.forEach((item)=>{
      attendanceCount[item.status]++;
      // console.log(item);
      });
    }); 
    console.log(attendanceCount);
    await nextTick();
    chartInst = new Chart(chartCanvas.value,{
      type: 'line',
      data: {
        labels: ['attendance','late','absent','leave'],
        datasets:[{
          label : 'Attendance Status',
          data: [attendanceCount.present, attendanceCount.late, attendanceCount.absent, attendanceCount.leave],
          backgroundColor: ['aqua','orangered','brown','skyblue']
        }]
      },
      options: {
        responsive: true,
        plugins: {
          legend: {position: 'bottom'} //범례(목록)를 하단으로
        }
      }
    });
  });
</script>

<style lang="scss" scoped>
  .chart-container{
  width: 100%;
  max-width: 400px;
  margin: auto;
  }
</style>