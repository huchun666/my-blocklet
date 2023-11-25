<script setup>
import { ref, onMounted } from 'vue';
const isEdit = ref(true);
const userName = ref('');
const userEmail = ref('');
const userPhone = ref('');
const nameErrorMessage = ref('');
const emailErrorMessage = ref('');
const phoneErrorMessage = ref('');

const saveOrEdit = () => {
  if (userName.value === '' || userEmail.value === '' || userPhone.value === '') return alert('请先填写信息');
  if (nameErrorMessage.value || emailErrorMessage.value || phoneErrorMessage.value) return alert('请先正确填写信息');
  isEdit.value = !isEdit.value;
  localStorage.setItem('userName', userName.value);
  localStorage.setItem('userEmail', userEmail.value);
  localStorage.setItem('userPhone', userPhone.value);
};

const handleChangeName = () => {
  if (userName.value === '') {
    return (nameErrorMessage.value = '请输入用户名');
  }
  nameErrorMessage.value = '';
};
const handleChangeEmail = () => {
  if (userEmail.value === '') {
    return (emailErrorMessage.value = '请输入邮箱');
  }
  const isPass = /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/.test(userEmail.value);
  if (!isPass) {
    return (emailErrorMessage.value = '请输入正确的邮箱');
  }
  emailErrorMessage.value = '';
};
const handleChangePhone = () => {
  if (userPhone.value === '') {
    return (phoneErrorMessage.value = '请输入手机号');
  }
  const isPass = /^[1][3,4,5,7,8][0-9]{9}$/.test(userPhone.value);
  if (!isPass) {
    return (phoneErrorMessage.value = '请输入正确的手机号');
  }
  phoneErrorMessage.value = '';
};

onMounted(() => {
  const tempUserName = localStorage.getItem('userName');
  const tempUserEmail = localStorage.getItem('userEmail');
  const tempUserPhone = localStorage.getItem('userPhone');
  if (tempUserName) {
    userName.value = tempUserName;
  }
  if (tempUserEmail) {
    userEmail.value = tempUserEmail;
  }
  if (tempUserPhone) {
    userPhone.value = tempUserPhone;
  }
});
</script>

<template>
  <div class="home-page">
    <div class="edit-box">
      <div class="label-box">
        <input
          class="edit-input"
          v-model="userName"
          @blur="handleChangeName"
          type="text"
          placeholder="请输入用户名"
          :disabled="!isEdit" />
        <div v-show="nameErrorMessage" class="error-message">{{ nameErrorMessage }}</div>
      </div>
      <div class="label-box">
        <input
          class="edit-input"
          v-model="userEmail"
          @blur="handleChangeEmail"
          type="text"
          placeholder="请输入邮箱"
          :disabled="!isEdit" />
        <div v-show="emailErrorMessage" class="error-message">{{ emailErrorMessage }}</div>
      </div>
      <div class="label-box">
        <input
          class="edit-input"
          v-model="userPhone"
          @blur="handleChangePhone"
          type="text"
          placeholder="请输入手机号"
          :disabled="!isEdit" />
        <div v-show="phoneErrorMessage" class="error-message">{{ phoneErrorMessage }}</div>
      </div>
    </div>
    <div class="button-box">
      <button class="save-button" @click="saveOrEdit">{{ isEdit ? '保存' : '编辑' }}</button>
    </div>
  </div>
</template>

<style>
.home-page {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  height: 100vh;
  padding: 0 40px;
  .edit-box {
    width: 100%;
    max-width: 400px;
    margin-bottom: 60px;
    .label-box {
      margin-bottom: 10px;
    }
    .edit-input {
      width: calc(100% - 20px);
      height: 44px;
      border-radius: 8px;
      padding: 0 10px;
      border: 1px solid #ddd;
    }
    .error-message {
      color: red;
    }
  }
  .save-button {
    width: 100px;
    height: 44px;
    border-radius: 10px;
    cursor: pointer;
  }
}
</style>
