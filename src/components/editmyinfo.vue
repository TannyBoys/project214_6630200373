<template>
  <div class="modal-backdrop">
    <div class="modal-content">
      <div class="card" style="background-color: #fffdf0">
        <div class="card-body">
          <form @submit.prevent="handleSubmit()">
            <div class="row">
              <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
                <label for="myId" class="form-label">รหัสนิสิต</label>
                <input type="text" id="myId" class="form-control" v-model.trim="editStudent.stdid" /> 
              </div>
              <div class="col-lg-8 col-md-8 col-sm-6 mt-2">
                <label for="myName" class="form-label">ชื่อ นามสกุล</label>
                <input type="text" id="myName" class="form-control" v-model.trim="editStudent.name" />
              </div>
              <div class="col-lg-6 col-md-5 col-sm-5 mt-2">
                <label for="faculty" class="form-label">คณะ</label>
                <input type="text" id="faculty" class="form-control" v-model.trim="editStudent.faculty" />
              </div>
              <div class="col-lg-6 col-md-5 col-sm-5 mt-2">
                <label for="major" class="form-label">สาขา</label>
                <input type="text" id="major" class="form-control" v-model.trim="editStudent.major" />
              </div>
              <div class="col-lg-6 col-md-5 col-sm-5 mt-2">
                <label for="location" class="form-label">มหาวิทยาลัย</label>
                <input type="text" id="location" class="form-control" v-model.trim="editStudent.location" /> 
              </div>
              <div class="col-lg-6 col-md-5 col-sm-5 mt-2">
                <label for="school" class="form-label">โรงเรียนเดิม</label>
                <input type="text" id="school" class="form-control" v-model.trim="editStudent.school" /> 
              </div>
              <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
                <label for="myYr" class="form-label">ชั้นปี</label>
                <select id="myYr" class="form-select" v-model="editStudent.yr">
                  <option value="1">ปี1</option>
                  <option value="2">ปี2</option>
                  <option value="3">ปี3</option>
                  <option value="4">ปี4</option>
                  <option value="5">เกินปี4</option>
                </select>
              </div>
              <div class="col-lg-4 col-md-4 col-sm-6 mt-2">
                <label for="" class="form-label">ภาคเรียน</label>
                <div class="form-check">
                  <input class="form-check-input" type="radio" id="sem1" value="ต้น" v-model.trim="editStudent.sem" />
                  <label class="form-check-label" for="sem1">ต้น</label>
                </div>
                <div class="form-check">
                  <input class="form-check-input" type="radio" id="sem2" value="ปลาย" v-model.trim="editStudent.sem" />
                  <label class="form-check-label" for="sem2">ปลาย</label>
                </div>
              </div>
              <div class="col-6 mt-4">
                <button type="submit" class="btn btn-warning">บันทึก</button>
                <button type="button" class="btn btn-secondary ms-2" @click="$emit('close')">ยกเลิก</button>
              </div>
            </div>
          </form>  
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "EditMyInfo",
  props: ["student"],
  data() {
    return {
      editStudent: { ...this.student },
    };
  },
  methods: {
    handleSubmit() {
      Object.assign(this.student, this.editStudent);
      fetch(`http://localhost:3000/student`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(this.student),
      })
      .then(() => {
        this.$emit("close");
      })
      .catch((err) => {
        alert("เกิดข้อผิดพลาด: " + err);
      });
    },
  },
};
</script>

<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 10px;
  max-width: 600px;
  width: 90%;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
}
</style>
