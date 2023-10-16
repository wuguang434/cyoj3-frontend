<template>
  <div id="userRegisterView" class="register-container">
    <h2 class="register-title">用户注册</h2>
    <a-form
      class="register-form"
      label-align="left"
      auto-label-width
      :model="form"
      @submit="handleSubmit"
    >
      <a-form-item field="userAccount" label="用户账号">
        <a-input v-model="form.userAccount" placeholder="请输入用户账号" />
      </a-form-item>
      <a-form-item field="userName" label="用户昵称">
        <a-input v-model="form.userName" placeholder="请输入用户昵称" />
      </a-form-item>
      <a-form-item field="userPassword" tooltip="密码不少于 8 位" label="密码">
        <a-input-password
          v-model="form.userPassword"
          placeholder="请输入密码"
        />
      </a-form-item>
      <a-form-item field="checkPassword" tooltip="确认密码" label="确认密码">
        <a-input-password
          v-model="form.checkPassword"
          placeholder="请再次输入密码"
        />
      </a-form-item>
      <a-form-item>
        <a-button type="primary" html-type="submit" style="width: 120px">
          点击注册
        </a-button>
      </a-form-item>
    </a-form>
  </div>
</template>

<script setup lang="ts">
import { reactive } from "vue";
import { UserControllerService, UserRegisterRequest } from "../../../generated";
import message from "@arco-design/web-vue/es/message";
import { useRouter } from "vue-router";

// 使用一个独立的类型定义来存储表单数据
type RegisterForm = {
  userAccount?: string;
  userName?: string;
  userPassword?: string;
  checkPassword?: string;
};

const form = reactive({
  userAccount: "",
  userName: "",
  userPassword: "",
  checkPassword: "",
} as RegisterForm);

const router = useRouter();

const handleSubmit = async () => {
  if (form.userPassword !== form.checkPassword) {
    message.error("两次输入的密码不一致");
    return;
  }

  const res = await UserControllerService.userRegisterUsingPost(form);
  if (res.code === 0) {
    message.success("注册成功");
    router.push({
      path: "/user/login",
      replace: true,
    });
  } else {
    message.error("注册失败，" + res.message);
  }
};
</script>
<style scoped>
.register-container {
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  padding: 40px;
  border-radius: 8px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
}

.register-title {
  text-align: center;
  margin-bottom: 24px;
  color: #333;
  font-weight: bold;
}

.register-form {
  max-width: 480px;
  margin: 0 auto;
}

a-input,
a-input-password {
  border-radius: 4px;
  transition: all 0.3s;
}

a-input:hover,
a-input-password:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

a-button {
  background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
  border: none;
  transition: all 0.3s;
}

a-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}
</style>
