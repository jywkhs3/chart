<template>
  <div class="app">
    <h1>Attendance Tracking Dashboard</h1>
    <div class="wrap">
      <MainAnalysis :students="students"/>
      <DateAnalysis :students="students"/>
      <StudentsAnalysis :students="students"/>
      <!-- <DrawChart :students="students"/> -->
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import StudentsAnalysis from './components/StudentsAnalysis.vue';
import DrawChart from './components/DrawChart.vue';
import DateAnalysis from './components/DateAnalysis.vue';
import MainAnalysis from './components/MainAnalysis.vue';

const students = ref([]);
const fetchData = async()=>{
  const response = await fetch('./attendanceData.json');
  // console.log(response);
  const data = await response.json();
  // console.log( data.students );
  students.value = data.students;
}
onMounted(fetchData);
</script>

<style lang="scss" scoped>

</style>