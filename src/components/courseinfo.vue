<template>
  <div class="container-course">
    <h1 class="mb-4">ผลการเรียนรายวิชา</h1>
    <img alt="TannyBoys Image" src="@/components/image/me2.jpg" width="300" />
    <AddCourseInfo/>
    <div class="courses">
      <div v-for="(course) in courses" :key="course.id" class="course-box">
        <div class="text">
          <h2>ชื่อวิชา: {{ course.name }}</h2>
          <h3>รหัสวิชา: {{ course.id }}</h3>
          <p class="text-font"> ปีการศึกษา {{ course.yr }} {{ course.semester }} <br>
          หน่วยกิต: {{course.credits}}</p>
          <h4 class="text-danger">เกรด: {{course.grade}}</h4>
          <button @click="openEditModal(course)" class="edit-btn">แก้ไขรายวิชา</button>
          <button class="delete-btn" @click="deleteCourse(course.id)">ลบรายวิชา</button>
        </div>
      </div>
    </div>

    <div v-if="editModalVisible" class="modal">
      <div class="modal-content">
        <EditCourseInfo :course="selectedCourse" @close="closeModal" />
      </div>
    </div>
  </div>
</template>

<script>
import EditCourseInfo from './editcourseinfo.vue'; 
import AddCourseInfo from './addcourseinfo.vue';
export default {
  name: "courseinfo",
  components: {EditCourseInfo, AddCourseInfo},
  data() {
    return {
      courses: [],
      editModalVisible: false,  
      selectedCourse: null, 
    };
  },
  methods: {
    fetchCourses() {
      fetch('http://localhost:3000/course')
        .then(response => response.json())
        .then(data => {
          this.courses = data;
        })
        .catch(error => console.error('Error:', error));
    },
    openEditModal(course) {
      this.selectedCourse = course;
      this.editModalVisible = true;
    },
    closeModal() {
      this.editModalVisible = false;
      this.selectedCourse = null;
    },
    deleteCourse(id) {
      if (confirm("คุณต้องการลบวิชานี้ใช่หรือไม่?")) {
        fetch(`http://localhost:3000/course/${id}`, {
          method: 'DELETE'
        })
        .then(() => {
          location.reload();
        })
        .catch(error => console.error('Error:', error));
      }
    },
  },
  mounted() {
    this.fetchCourses();
  },
};
</script>

<style scoped>
.container-course {
  margin: 1rem;
  padding: 1rem;
  box-shadow: 0 2px 7px rgba(0, 0, 0, 0.2);
  background-color: cornsilk;
  display: flex;
  flex-direction: column;
  align-items: center;
}

img {
  border-radius: 8px;
  margin: 0rem 2rem 2rem 2rem;
  ;
}

.text-font{
  font-size: 20px;
}

.courses {
  display: block;
}

.course-box {
  background-color: #fffdf0;
  padding: 1rem;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  margin-bottom: 1rem;
  width: 100%;
  max-width: 100%;
}

.edit-btn {
  font-size: 16px;
  background-color: #e9c845;
  margin-top: 2px;
  border: none;
  padding: 0.5rem 1rem;
  cursor: pointer;
  border-radius: 5px;
}

.delete-btn{
  font-size: 16px;
  background-color: #dc3545;
  color: white;
  margin-top: 2px;
  margin-left: 5px;
  border: none;
  padding: 0.5rem 1rem;
  cursor: pointer;
  border-radius: 5px;
}

.text {
  font-size: 14px;
  color: #333;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 2rem;
  border-radius: 8px;
  width: 60%;
  max-width: 800px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}
</style>
