<template>
  <div>
    <main-header navsel="front"></main-header>
    <div class="header">
      <h3><i class="fas fa-box"></i> ทำการจอง</h3>
    </div>
    <div class="container-fluid">
      <div class="container">
        <div class="blog-wrapper">
          <h4><i class="far fa-clipboard"></i> รายละเอียดการทำธุรกรรม</h4>
          <br />
          <span class="font2">
            <form v-on:submit.prevent="createBorrow">
              <div class="row">
                <div class="col">
                  <label for="input1">ชื่อ</label>
                  <div class="input-group mb-3">
                    <div class="input-group-prepend">
                      <span class="input-group-text" id="basic-addon1"
                        ><i class="far fa-user"></i
                      ></span>
                    </div>
                    <input
                      required
                      type="text"
                      class="form-control"
                      placeholder="Name"
                      name="nameLend"
                      id="nameLend"
                      v-model="borrow.nameLend = this.user.name"
                      disabled
                    />
                  </div>
                </div>
                <div class="col">
                  <label for="input1">วันที่ทำรายการ</label>
                  <div class="input-group mb-3">
                    <div class="input-group-prepend">
                      <span class="input-group-text" id="basic-addon1"
                        ><i class="far fa-calendar-alt"></i
                      ></span>
                    </div>
                    <input
                      required
                      type="date"
                      class="form-control"
                      placeholder="Date"
                      v-model="data.date"
                      disabled
                    />
                  </div>
                </div>
              </div>
              <div class="row">
                <div class="col">
                  <label for="input1">ยืม</label>
                  <div class="input-group mb-3">
                    <div class="input-group-prepend">
                      <span class="input-group-text" id="basic-addon1"
                        ><i class="far fa-calendar-alt"></i
                      ></span>
                    </div>
                    <input
                      required
                      type="date"
                      class="form-control"
                      placeholder="Date"
                      v-model="borrow.dateLend"
                    />
                  </div>
                </div>
                <div class="col">
                  <label for="input1">คืน</label>
                  <div class="input-group mb-3">
                    <div class="input-group-prepend">
                      <span class="input-group-text" id="basic-addon1"
                        ><i class="far fa-calendar-check"></i
                      ></span>
                    </div>
                    <input
                      required
                      type="date"
                      class="form-control"
                      placeholder="Date"
                      v-model="borrow.dateReturn"
                    />
                  </div>
                </div>
              </div>
              <div class="row">
                <div class="col">
                  <label for="input1">เลือกรถ</label>
                  <div class="input-group mb-3">
                    <div class="input-group-prepend">
                      <span class="input-group-text" id="basic-addon1"
                        ><i class="fas fa-boxes"></i
                      ></span>
                    </div>
                    <select v-model="borrow.equipment" required>
                      <option disabled value="">เลือกรถ</option>
                      <option v-for="blog in blogs" v-bind:key="blog.id">
                        {{ blog.title }}
                      </option>
                    </select>
                  </div>
                </div>
                <div class="col">
                  <label for="input1">จำนวน/คัน</label>
                  <div class="input-group mb-3">
                    <div class="input-group-prepend">
                      <span class="input-group-text" id="basic-addon1"
                        ><i class="fas fa-sort-numeric-up"></i
                      ></span>
                    </div>
                    <input
                      required
                      type="number"
                      class="form-control"
                      placeholder="Number"
                      v-model="borrow.number"
                    />
                  </div>
                </div>
              </div>
              <div
                class="shadow-sm p-3 mb-5 bg-light rounded"
                style="padding: 0.5rem !important"
              >
                
              </div>
              <div class="row">
                <div class="col">
                  <button
                    type="submit"
                    class="btn btn-success"
                    style="width: 100%"
                  >
                    <i class="fas fa-clipboard-check"></i> ยืนยัน
                  </button>
                </div>
                <div class="col">
                  <button
                    class="btn btn-danger"
                    type="button"
                    style="width: 100%"
                    v-on:click="navigateTo('/front')"
                  >
                    <i class="fas fa-times-circle"></i> ยกเลิก
                  </button>
                </div>
              </div>
            </form>
          </span>
        </div>
        <div class="footer"></div>
      </div>
    </div>
  </div>
</template>
<script>
import { mapState } from "vuex";
import BorrowsService from "@/services/BorrowsService";
import BlogsService from "@/services/BlogsService";
import UsersService from "@/services/UsersService";

export default {
  data() {
    return {
      users: [],
      blogs: [],
      borrow:{
        nameLend: "",
        dateLend: "",
        dateReturn: "",
        equipment: "",
        number: "",
        status: "pending review",
      },
      data: { date: new Date().toISOString().substr(0, 10) },
    };
  },
  computed: {
    ...mapState(["isUserLoggedIn", "user"]),
    name: function() {
      return this.user.name;
    },
  },
  methods: {
    async createBorrow() {
      try {
        alert("จองรถสำเร็จ");
        await BorrowsService.post(this.borrow);
        this.$router.push({
          name: "borrow-status",
        });
      } catch (err) {
        console.log(err);
      }
    },
    navigateTo(route) {
      this.$router.push(route);
    },
  },
  async created() {
    this.users = (await UsersService.index()).data;
    this.blogs = (await BlogsService.index()).data;
  },
};
</script>>

<style scoped>
.header {
  margin-left: auto;
  margin-right: auto;
  margin-top: 0px;
  padding: 10px;
  position: relative;
  background-color: #ececec;
  border-bottom: 1px solid rgba(255, 255, 255, 0.5);
  box-shadow: 0 -2px 1px rgba(0, 0, 0, 0.1) inset;
  text-shadow: 1px 1px 1px #fff;
}
.categories {
  margin-top: 10px;
  padding: 0;
  width: 100%;
}
.blog-wrapper {
  margin-top: 20px;
  padding: 40px;
  height: 100%;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.blog-tab {
  padding: 1px;
  background-color: #d3d3d3;
  text-align: left;
  text-indent: 0.5em;
}
.footer {
  margin-top: 50px;
}
</style>
<!-- eng -->