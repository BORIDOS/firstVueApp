<template>
  <div class="app-container">
    <h1>📝 Task Manager ของฉัน</h1>
    <TaskList/>
  </div>
</template>

<script>
import TaskList from './components/TaskList.vue';

  //พื้นที่สำหรับเขียน JavaScript (ดึง API, จัดการข้อมูล) จะอยู่ตรงนี้
</script>

<script setup>
  import TaskList from './components/TaskList.vue';
</script>

<style>
  /* พื้นที่สำหรับตกแต่งความสวยงาม (CSS) ของหน้านี้ */
  .app-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: sans-serif;
}
</style>