<script setup>
import DynamicForm from './components/DynamicForm.vue';

const formData = {
  name: '张三',
  age: 18,
  sex: '男'
};
const formConfig = [
  {
    id: '1',
    label: '姓名',
    field: 'name',
    componentName: 'ElInput',
    componentProps: {
      required: true,
      placeholder: '请输入姓名'
    }
  },
  {
    id: '2',
    label: '年龄',
    field: 'age',
    componentName: 'ElInput',
    componentProps: {
      required: true,
      placeholder: '请输入年龄'
    }
  },
  {
    id: '3',
    componentName: 'ElRadio',
    field: 'sex',
    required: true,
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
