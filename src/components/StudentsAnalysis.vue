<template>
  <div class="student-analysis">
    <h2>"Student Attendance Status"</h2>
    <select v-model="selectedStudent">
      <option v-for="list in students" :key="list.id" :value="list">{{ list.name }}</option>
    </select>
      <div class="chart-container">
        <!-- <p>name : {{ selectedStudent.name }}</p> -->
        <canvas ref="chartCanvas"></canvas> 
      </div>
  </div>
</template>

<script setup>
import { Chart, registerables } from 'chart.js';
import { ref, watch } from 'vue';

  defineProps({
    students: Array
  });

  const selectedStudent = ref('');
  const chartCanvas = ref(null);
  let chartInst = null;
  Chart.register(...registerables);

  watch(selectedStudent,()=>{
        // console.log(selectedStudent.value.attendance);
    if(chartInst){
      chartInst.destroy(); //destroy: 만든차트를 삭제시켜주는 명령
    }
    const attendanceCount = {present:0, leave:0, absent:0, late:0};
    selectedStudent.value.attendance.forEach((list)=>{
      attendanceCount[list.status]++;
    });
    // console.log(attendanceCount);
    chartInst = new Chart(chartCanvas.value,{
      type: 'doughnut',
      data: {
        labels: ['attendance','late','absent','leave'],
        datasets:[{
          label : 'Attendance Status',
          data: [attendanceCount.present, attendanceCount.late, attendanceCount.absent, attendanceCount.leave],
          backgroundColor: ['yellowgreen','orange','brown','blue']
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
  select{
    width: 60%;
    height: 2rem;
    font-size: 1rem;
    margin-bottom: 2rem;
  }
</style>