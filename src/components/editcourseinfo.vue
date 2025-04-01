<template>
  <div class="card" style="background-color: #fffdf0">
    <div class="card-body">
      <form @submit.prevent="handleSubmit()">
        <div class="row">
          <div class="col-lg-8 col-md-8 col-sm-6 mt-2">
            <label for="courseName" class="form-label">ชื่อวิชา</label>
            <input type="text" id="courseName" class="form-control" v-model.trim="editCourse.name" />
          </div>
          <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
            <label for="courseId" class="form-label">รหัสวิชา</label>
            <input type="text" id="courseId" class="form-control" v-model.trim="editCourse.id" disabled /> 
          </div>
          <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
            <label for="courseYr" class="form-label">ปีการศึกษา</label>
            <input type="number" id="courseYr" class="form-control" v-model.trim="editCourse.yr" />
          </div>
          <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
            <label for="credit" class="form-label">หน่วยกิต</label>
            <input type="number" id="credit" class="form-control" v-model.trim="editCourse.credits" />
          </div>
          <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
            <label for="grade" class="form-label">เกรด</label>
            <select id="grade" class="form-select" v-model="editCourse.grade">
              <option value="A">A</option>
              <option value="B+">B+</option>
              <option value="B">B</option>
              <option value="C+">C+</option>
              <option value="C">C</option>
              <option value="D+">D+</option>
              <option value="D">D</option>
              <option value="F">F</option>
              <option value="N">N</option>
            </select>
          </div>
          <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
            <label for="courseSem" class="form-label">ภาคเรียน</label>
            <select id="courseSem" class="form-select" v-model="editCourse.semester">
              <option value="ภาคต้น">ภาคต้น</option>
              <option value="ภาคปลาย">ภาคปลาย</option>
            </select>
          </div>
          <div class="col-2 mt-4 d-flex align-item">
          </div>
        </div>
        <button type="submit" class="edit-btn">บันทึก</button>
        <button @click="$emit('close')" class="close-btn">ปิด</button>
      </form>  
    </div>
  </div>
</template>

<script>
export default {
  name: "EditCourseInfo",
  props: ["course"],
  data() {
    return {
      editCourse: { ...this.course },
      addOK: false,
      addErr: false,
      addMessage: null,
    };
  },
  methods: {
    handleSubmit() {
      Object.assign(this.course, this.editCourse);

      fetch(`http://localhost:3000/course`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(this.course),
      })
        .then(() => {
          this.addOK = true;
          this.addErr = false;
          
          this.$emit('close');
        })
        .catch((err) => {
          this.addErr = true;
          this.addOK = false;
        });
    },
  },
};
</script>

<style>
.edit-btn {
  font-size: 16px;
  background-color: #e9c845;
  margin-top: 2rem;
  border: none;
  padding: 0.5rem 1rem;
  cursor: pointer;
  border-radius: 5px;
}

.close-btn{
  font-size: 16px;
  background-color: #6c757d;
  color: white;
  margin-top: 2rem;
  margin-left: 5px;
  border: none;
  padding: 0.5rem 1rem;
  cursor: pointer;
  border-radius: 5px;
}
</style>