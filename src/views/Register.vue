<template>
  <div class="register">
    <section class="form_container">
      <div class="manage_tip">
        <span class="title">後臺管理系統</span>
      </div>
      <el-form
        :model="registerUser"
        :rules="rules"
        class="registerForm"
        ref="registerForm"
        label-width="80px"
      >
        <el-form-item label="用戶名稱" prop="name">
          <el-input v-model="registerUser.name" placeholder="請輸入用戶名"></el-input>
        </el-form-item>
        <el-form-item label="電子信箱" prop="email">
          <el-input v-model="registerUser.email" placeholder="請輸入電子信箱"></el-input>
        </el-form-item>
        <el-form-item label="密碼" prop="password">
          <el-input v-model="registerUser.password" placeholder="請輸入密碼" type="password"></el-input>
        </el-form-item>
        <el-form-item label="確認密碼" prop="password2">
          <el-input v-model="registerUser.password2" placeholder="請確認密码" type="password"></el-input>
        </el-form-item>
        <el-form-item label="選擇身份">
          <el-select v-model="registerUser.identity" placeholder="請選擇身分">
            <el-option label="管理員" value="manager"></el-option>
            <el-option label="員工" value="employee"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" class="submit_btn" @click="submitForm('registerForm')">註冊</el-button>
        </el-form-item>
      </el-form>
    </section>
  </div>
</template>

<script>
export default {
  name: "register",
  data() {
    var validatePass2 = (rule, value, callback) => {
      if (value !== this.registerUser.password) {
        callback(new Error("兩次輸入密碼不一致!"));
      } else {
        callback();
      }
    };
    return {
      registerUser: {
        name: "",
        email: "",
        password: "",
        password2: "",
        identity: ""
      },
      rules: {
        name: [
          { required: true, message: "用戶名不能為空", trigger: "change" },
          { min: 2, max: 30, message: "長度在2到30個字元", trigger: "blur" }
        ],
        email: [
          {
            type: "email",
            required: true,
            message: "郵箱格式不正確",
            trigger: "blur"
          }
        ],
        password: [
          { required: true, message: "密碼不能為空", trigger: "blur" },
          { min: 6, max: 30, message: "長度在6到30個字元", trigger: "blur" }
        ],
        password2: [
          { required: true, message: "確認密碼不能為空", trigger: "blur" },
          {
            min: 6,
            max: 30,
            message: "長度在6到30個字元",
            trigger: "blur"
          },
          { validator: validatePass2, trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.$axios
            .post("/api/users/register", this.registerUser)
            .then(res => {
              // 註冊成功
              this.$message({
                message: "註冊成功！",
                type: "success"
              });
              this.$router.push("/login");
            });
        // } else {
        //   console.log("error submit!!");
        //   return false;
        }
      });
    }
  }
};
</script>


<style scoped>
.register {
  position: relative;
  width: 100%;
  height: 100%;
  background: url(../assets/bg.jpg) no-repeat center center;
  background-size: 100% 100%;
}
.form_container {
  width: 370px;
  height: 210px;
  position: absolute;
  top: 10%;
  left: 34%;
  padding: 25px;
  border-radius: 5px;
  text-align: center;
}
.form_container .manage_tip .title {
  font-family: "Microsoft YaHei";
  font-weight: bold;
  font-size: 26px;
  color: #fff;
}
.registerForm {
  margin-top: 20px;
  background-color: #fff;
  padding: 20px 40px 20px 20px;
  border-radius: 5px;
  box-shadow: 0px 5px 10px #cccc;
}

.submit_btn {
  width: 100%;
}
</style>



