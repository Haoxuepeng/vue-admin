<template>
  <div id="login">
    <div class="login-wrap">
      <ul class="tab-menu">
        <li
          @click="menuTabClick"
          v-for="(item, index) in menuTab"
          :key="index"
          :class="{ current: isActive == index }"
        >
          {{ item.name }}
        </li>
      </ul>
      <el-form
        :model="ruleForm"
        status-icon
        :rules="rules"
        ref="ruleForm"
        label-width="100px"
        class="demo-ruleForm"
      >
        <el-form-item label="邮箱" prop="email">
          <el-input
            type="text"
            v-model="ruleForm.email"
            autocomplete="off"
          ></el-input>
        </el-form-item>

        <el-form-item label="密码" prop="password">
          <el-input
            type="password"
            v-model="ruleForm.password"
            autocomplete="off"
          ></el-input>
          </el-form-item>
          <!-- 重复密码 -->
          <el-form-item label="重复密码" prop="passwords" v-show="flag">
          <el-input
            type="password"
            v-model="ruleForm.passwords"
            autocomplete="off"
          ></el-input>
        </el-form-item>

        <el-form-item label="验证码" prop="code">
          <el-row :gutter="10">
            <el-col :span="16"
              > <el-input v-model.number="ruleForm.code"></el-input></el-col>
            <el-col :span="5"
              >
              <el-button size='small'  type="danger" class="block">发送验证码</el-button>
              </el-col>
          </el-row>
         
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')"
            >提交</el-button
          >
          <el-button @click="resetForm('ruleForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import { validateEmail } from '@/utils/valitedata'
export default {
  name: 'Login',
  props: {},
  data() {
        // 验证账号
    var checEmail = (rule, value, callback) => {
      var reg = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;
      if (!value) {
        return callback(new Error('邮箱不能为空'));
      } else if (validateEmail(value)) {
        return callback(new Error('邮箱格式不正确'));
      } else {
        callback();
      }
    };
      // 验证密码
    var validatePass = (rule, value, callback) => {
      var reg = /^[a-zA-Z]{1}([a-zA-Z0-9]|[._]){4,19}$/;
      if (value === '') {
        callback(new Error('请输入密码'));
      } else if (!reg.test(value)) {
        return callback(new Error('只能输入5-20个以字母开头密码'));
      } else {
        callback();
      }
    };
    // 验证重复密码
     var validatePassWord = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码'));
      } else if (value!=this.ruleForm.password) {
        return callback(new Error('两次输入的密码不一致,请重新输入'));
      } else {
        callback();
      }
    };
        // 验证验证码
    var checkCode = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入验证码'));
      } else {
        callback();
      }
    };
    return {
      flag:false,
      menuTab: [{ name: '登录' }, { name: '注册' }],
      isActive: 0,
      ruleForm: {
        email: '',
        password: '',
        passwords:'',
        code: '',
      },
      rules: {
        email: [{ validator: checEmail, trigger: 'blur' }],
        password: [{ validator: validatePass, trigger: 'blur' }],
        passwords: [{ validator: validatePassWord, trigger: 'blur' }],
        code: [{ validator: checkCode, trigger: 'blur' }],
      },
    };
  },

  computed: {},

  beforeMount() {},

  mounted() {},

  methods: {
    menuTabClick() {
      if (this.isActive == 0) {
        this.flag=true
        this.isActive = 1;
      } else {
        this.flag=false
        this.isActive = 0;
      }
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!');
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
  },

  watch: {},
};
</script>
<style scoped lang="scss">
ul,
li {
  list-style: none;
}

#login {
  height: 100vh;
  background-color: #344a5f;
}

.login-wrap {
  width: 330px;
  margin: auto;
}
.tab-menu {
  text-align: center;
  li {
    display: inline-block;
    width: 88px;
    line-height: 36px;
    font-size: 14px;
    color: #fff;
    border-radius: 2px;
    cursor: pointer;
  }
  .current {
    background-color: rgba(0, 0, 0, 0.1);
  }
}
.block{
  display: inline-block;
}
</style>
