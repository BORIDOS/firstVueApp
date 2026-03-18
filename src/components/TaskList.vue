<template>
    <div class="task-list-container">
        <h2>📌 รายการ Task ทั้งหมด</h2>

        <!-- วนลูปแสดงข้อมูลจากตัวแปร tasks -->
        <ul>
            <li v-for="task in tasks" :key="task.id" class="task-item">
                <span :class="{ completed: task.isCompleted}">
                    {{ task.title }}
                </span>
            </li>
        </ul>
    </div>
</template>

<script setup>
    import { ref } from 'vue';

    // สร้างข้อมูลจำลอง (Mock Data) ไปก่อน เดี๋ยว Step ถัดไปเราจะดึงจาก API จริง
    const tasks = ref ([
        { id: 1, title: 'ออกแบบ Database', isCompleted: true },
        { id: 2, title: 'สร้าง Vue Component', isCompleted: false },
    ]);
</script>

<style scoped>
.task-list-container {
    margin-top: 20px;
    background-color: #f9f9f9;
    padding: 15px;
    border-radius: 8px;
}
ul {
    list-style-type: none;
    padding: 0;
}
.task-item {
    padding: 10px;
    border-bottom: 1px solid #ddd;
    font-size: 18px;
}
/* ถ้าทำเสร็จแล้ว ให้ขีดฆ่าตัวหนังสือ */
.completed {
    text-decoration: line-through;
    color: #888;
}
</style>