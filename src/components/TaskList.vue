<template>
    <div class="task-list-container">
        <h2>📌 รายการ Task ทั้งหมด</h2>

        <!-- ส่วนที่เพิ่มใหม่ 1: ฟอร์มสำหรับเพิ่ม Task -->
        <div class="add-task-form">
            <!-- v-model จะเชื่อมช่องนี้เข้ากับตัวแปร newTaskTitle -->
            <input v-model="newTaskTitle" type="text" placeholder="พิมพ์งานที่ต้องทำ..." @keyup.enter="addTask" />
            <button @click="addTask">เพิ่มงาน</button>
        </div>

        <ul>
            <li v-for="task in tasks" :key="task.id" class="task-item">
                <span :class="{ completed: task.isCompleted }">
                    {{ task.title }}
                </span>
            </li>
        </ul>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const tasks = ref([]);

// ส่วนที่เพิ่มใหม่ 2: ตัวแปรเก็บข้อความ และฟังก์ชันส่ง API
const newTaskTitle = ref('');

const fetchTasks = async () => {
    try {
        const response = await axios.get('http://localhost:5068/tasks');
        tasks.value = response.data;
    } catch (error) {
        console.error('เกิดข้อผิดพลาดในการดึงข้อมูล:', error);
    }
};

// ฟังก์ชันส่งข้อมูลไปให้ C# Backend
const addTask = async () => {
    // เช็คก่อนว่าไม่ได้เผลอกดส่งช่องว่างเปล่าๆ มา
    if (newTaskTitle.value.trim() === '') return;

    try {
        // สั่งพนักงาน Axios วิ่งเอาข้อมูลไปส่งแบบ POST
        const response = await axios.post('http://localhost:5068/tasks', {
            title: newTaskTitle.value,
            isCompleted: false
        });

        // พอ Backend บันทึกเสร็จและตอบกลับมา เราก็เอา Task ใหม่ยัดใส่กล่อง tasks เพื่อแสดงบนเว็บ
        tasks.value.push(response.data);

        // ล้างช่องกรอกข้อมูลให้ว่างเพื่อเตรียมพิมพ์งานชิ้นต่อไป
        newTaskTitle.value = '';
    } catch (error) {
        console.error('เกิดข้อผิดพลาดในการเพิ่มข้อมูล:', error);
    }
};

onMounted(() => {
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

.completed {
    text-decoration: line-through;
    color: #888;
}

/* ส่วนที่เพิ่มใหม่ 3: CSS ตกแต่งฟอร์ม */
.add-task-form {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
}

input {
    flex: 1;
    padding: 8px;
    font-size: 16px;
}

button {
    padding: 8px 16px;
    background-color: #4CAF50;
    color: white;
    border: none;
    cursor: pointer;
    font-size: 16px;
}

button:hover {
    background-color: #45a049;
}
</style>