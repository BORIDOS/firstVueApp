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
    import { onMounted, ref } from 'vue';
    import axios from 'axios';
    // สร้างข้อมูลจำลอง (Mock Data) ไปก่อน เดี๋ยว Step ถัดไปเราจะดึงจาก API จริง
    const tasks = ref ([]);

    const fetchTasks = async () => {
        try {
            const response = await  axios.get ('http://localhost:5068/tasks');
            tasks.value = response.data; // เอาข้อมูลที่ได้มาใส่ในกล่อง tasks
        } catch (error) {
            console.error ('เกิดข้อผิพลาด',error);
        }
    };

    // onMounted คือคำสั่งบอก Vue ว่า "พอวาดหน้าเว็บเสร็จปุ๊บ ให้เรียกฟังก์ชันดึงข้อมูลทันทีเลยนะ!"
    onMounted (() => {
        fetchTasks();
    });
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