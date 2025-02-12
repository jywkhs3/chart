<template>
  <div class="date-analysis">
    <h2>"Daily Attendance Status"</h2>
    <input type="date" v-model="selectedDate"/>
    <div class="chart-container" v-if="isDate">
      <canvas ref="chartCanvas"></canvas>
    </div>
    <!-- 데이터가 없으면 -->
    <p v-else>*해당 날짜의 출결 데이터가 없습니다.*</p>
  </div>
</template>

<script setup>
import { Chart, registerables } from 'chart.js';
import { computed, nextTick, ref, watch } from 'vue';

  const props = defineProps({
    students : Array
  });
  const selectedDate = ref('');
  const chartCanvas = ref(null);
  const isDate = ref(null);

  let chartInst = null;
  Chart.register(...registerables);
  //선택한 날짜가 변경되면
  watch(selectedDate, async()=>{
    // console.log(selectedDate.value); //출력값 : 2025-02-11
    const attendanceCount = {present:0, leave:0, absent:0, late:0};
    isDate.value = false;
    // console.log(props.students);
    props.students.forEach((list)=>{
      // console.log(list);
      list.attendance.forEach((item)=>{
        // console.log(item);
        if(item.date === selectedDate.value){
          attendanceCount[item.status]++;
          isDate.value = true;
        }
      });
    });
    // console.log('출결데이터',attendanceCount);
    //destroy: 만든차트를 삭제시켜주는 명령
    if(chartInst){
      chartInst.destroy(); 
    }
    //데이터가 없다면 차트를 생성할 필요가 없음
    if( !isDate.value ) return;
    //DOM업데이트가 되면 재갱신
    await nextTick();

    //차트 그리기 bar
    chartInst = new Chart(chartCanvas.value,{
      type: 'bar',
      data: {
        labels: ['출석','지각','결석','조퇴'],
        datasets:[{
          label : '일자별 출결 현황 데이터',
          data: [attendanceCount.present, attendanceCount.late, attendanceCount.absent, attendanceCount.leave],
          backgroundColor: ['pink','tomato','beige','skyblue']
        }]
      },
      options: {
        responsive: true,
        plugins: {
          legend: {position: 'bottom'} //범례위치 변경
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