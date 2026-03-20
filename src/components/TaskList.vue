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
                <button class="delete-btn" @click="deleteTask(task.id)">ลบ 🗑️</button>
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

    const deleteTask = async (id) => {
        if (!confirm('คุณแน่ใจหรือไม่ว่าต้องการลบงานนี้?')) return;

        try {
            await axios.delete(`http://localhost:5068/tasks/${id}`);

            tasks.value = tasks.value.filter(task => task.id !== id);
        }catch (error) {
            console.error ('เกิดข้อผิดพลาดในการลบข้อมูล:', error);
        }
    }

    onMounted(() => {
        fetchTasks();
    });
</script>

<style scoped>
/* ตกแต่งกล่องหลักให้เหมือนการ์ด */
.task-list-container {
    margin-top: 20px;
    background-color: #ffffff;
    padding: 25px;
    border-radius: 12px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    /* เพิ่มเงาให้ดูมีมิติ */
}

/* ตกแต่งหัวข้อ */
h2 {
    color: #333;
    text-align: center;
    margin-bottom: 20px;
}

/* ตกแต่งฟอร์มเพิ่มข้อมูล */
.add-task-form {
    display: flex;
    gap: 10px;
    margin-bottom: 25px;
}

input {
    flex: 1;
    padding: 12px;
    font-size: 16px;
    border: 1px solid #ccc;
    border-radius: 6px;
    transition: border-color 0.3s;
}

input:focus {
    outline: none;
    border-color: #4CAF50;
    /* เปลี่ยนสีขอบเมื่อคลิกพิมพ์ */
}

button {
    padding: 10px 20px;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    font-size: 16px;
    font-weight: bold;
    transition: transform 0.1s, background-color 0.3s;
}

button:hover {
    background-color: #45a049;
}

button:active {
    transform: scale(0.95);
}

/* เอฟเฟกต์ปุ่มบุ๋มตอนกด */

/* ตกแต่งรายการ Task */
ul {
    list-style-type: none;
    padding: 0;
}

.task-item {
    display: flex;
    align-items: center;
    padding: 15px 10px;
    border-bottom: 1px solid #eee;
    font-size: 18px;
    color: #555;
    transition: background-color 0.2s;
}

.task-item:hover {
    background-color: #f9f9f9;
    /* ไฮไลท์สีพื้นหลังเมื่อเอาเมาส์ชี้ */
}

.completed {
    text-decoration: line-through;
    color: #aaa;
}

/* ตกแต่งปุ่มลบ */
.delete-btn {
    background-color: #ff4c4c;
    padding: 6px 12px;
    margin-left: auto;
    font-size: 14px;
}

.delete-btn:hover {
    background-color: #e60000;
}
</style>