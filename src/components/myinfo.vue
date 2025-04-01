<template>
  <div class="container_myinfo">
    <img alt="TannyBoys Image" src="@/components/image/me1.jpg" class="image" width="300" />
    <div class="text col-lg-6 col-md-5 col-sm-6">
      <h1>ข้อมูลนิสิต</h1>
      รหัสนิสิต {{ student.stdid }} ชื่อ {{ student.name }} <br>
      คณะ {{ student.faculty }} สาขา {{ student.major }} <br>
      ศึกษาอยู่ชั้นปีที่ {{ student.yr }} เทอม {{ student.sem }} <br>
      {{ student.location }} <br>
      โรงเรียนเดิม คือ {{ student.school }} <br>
      <button @click="editMode = true" class="edit-btn">แก้ไขข้อมูล</button>
    </div>
  </div>
  
  <!-- ใช้ popup -->
  <EditMyInfo v-if="editMode" :student="student" @close="editMode = false" />
</template>

<script>
import EditMyInfo from "./editmyinfo.vue";

export default {
  name: "myinfo",
  components: { EditMyInfo },
  data() {
    return {
      student: {},
      editMode: false,
    };
  },
  mounted() {
    this.fetchStudent();
  },
  methods: {
    async fetchStudent() {
      try {
        const response = await fetch("http://localhost:3000/student");
        this.student = await response.json();
      } catch (error) {
        console.error("Error fetching student data:", error);
      }
    },
  },
};
</script>

<style scoped>
.container_myinfo {
  margin: 1rem;
  box-shadow: 0 2px 7px;
  background-color: cornsilk;
  display: flex;
  align-items: center;
}

.text {
  color: black;
  padding-bottom: 1rem;
  font-size: 25px;
  margin-left: 10px;
}

.image {
  border-radius: 8px;
  margin: 2rem;
}

.edit-btn {
  font-size: 16px;
  margin-top: 10px;
  padding: 5px 10px;
  background: #dc3545;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.popup-content {
  background: white;
  padding: 20px;
  border-radius: 10px;
  width: 50%;
  max-width: 500px;
  position: relative;
}

.close-btn {
  position: absolute;
  top: 10px;
  right: 15px;
  font-size: 24px;
  cursor: pointer;
}
</style>
