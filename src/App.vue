<script setup>
import DynamicForm from './components/DynamicForm.vue';

const formData = {
  name: '张三',
  age: 18,
  sex: '男',
  hobby: ['打篮球', '打游戏'],
  address: {
    province: '浙江省',
    city: '杭州市',
    district: '西湖区'
  }
};
const formConfig = [
  {
    type: 'input',
    label: '姓名',
    field: 'name',
    required: true,
    placeholder: '请输入姓名'
  },
  {
    type: 'input',
    label: '年龄',
    field: 'age',
    required: true,
    placeholder: '请输入年龄'
  },
  {
    type: 'radio',
    label: '性别'
  }
];
</script>

<template>
  <span>动态表单设计</span>
  <dynamic-form :form-data="formData" :config="formConfig"></dynamic-form>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
