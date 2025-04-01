<template>
  <div class="text-center">
    <button @click="showModal = true" class="add-btn">เพิ่มรายวิชา</button>
  
    <div v-if="showModal" class="modal d-block" tabindex="-1" role="dialog">
      <div class="modal-dialog" role="document">
        <div class="modal-content p-3">
          <div class="modal-header">
            <h5 class="modal-title">เพิ่มวิชาใหม่</h5>
          </div>
          <div class="modal-body">
            <div class="form-group">
            <div class="form-group">
              <label>รหัสวิชา</label>
              <input v-model="newCourse.id" type="text" class="form-control" />
            </div>
              <label>ชื่อวิชา</label>
              <input v-model="newCourse.name" type="text" class="form-control" />
            </div>
            <div class="form-group">
              <label>ปีการศึกษา</label>
              <input v-model="newCourse.yr" type="number" class="form-control" />
            </div>
            <div class="form-group">
              <label>ภาคการศึกษา</label>
              <select v-model="newCourse.semester" class="form-control">
                <option value="">-- เลือกภาคการศึกษา --</option>
                <option value="ภาคต้น">ภาคต้น</option>
                <option value="ภาคปลาย">ภาคปลาย</option>
              </select>
            </div>
            <div class="form-group">
              <label>หน่วยกิต</label>
              <input v-model="newCourse.credits" type="number" class="form-control" />
            </div>
            <div class="form-group">
              <label>Grade</label>
              <select v-model="newCourse.grade" class="form-control">
                <option value="">-- เลือกเกรด --</option>
                <option value="A">A</option>
                <option value="B+">B+</option>
                <option value="B">B</option>
                <option value="C+">C+</option>
                <option value="C">C</option>
                <option value="D+">D+</option>
                <option value="D">D</option>
                <option value="F">F</option>
              </select>
            </div>
          </div>
          <div class="modal-footer">
            <button class="btn btn-success" @click="addCourse">เพิ่ม</button>
            <button class="btn btn-secondary" @click="showModal = false">ยกเลิก</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showModal: false,
      newCourse: {
        semester: '',
        yr: '',
        id: '',
        name: '',
        credits: '',
        grade: ''
      }
    };
  },
  methods: {
  addCourse() {
  if (!this.newCourse.id || this.newCourse.id.trim() === '') {
    alert("เกิดข้อผิดพลาด: กรุณากรอก ID");
    return;
  }

  fetch('http://localhost:3000/course')
    .then(response => response.json())
    .then(courses => {
      if (courses.some(course => course.id === this.newCourse.id)) {
        alert("เกิดข้อผิดพลาด: ID นี้มีอยู่แล้วในระบบ!");
        return;
      }

      return fetch('http://localhost:3000/course', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(this.newCourse)
      });
    })
    .then(response => response ? response.json() : null)
    .then(data => {
      if (data) {
        this.$emit('course-added', data);
        this.showModal = false;
        location.reload();
      }
    })
    .catch(error => console.error('Error:', error));
}
}
};
</script>

<style>
.add-btn{
  font-size: 16px;
  background-color: #0dcaf0;
  margin-bottom: 1rem;
  border: none;
  padding: 0.5rem 1rem;
  cursor: pointer;
  border-radius: 5px;
}

.modal {
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
