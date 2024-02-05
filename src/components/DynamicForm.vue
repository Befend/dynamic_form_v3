<script setup>
import {
  ElForm,
  ElCascader,
  ElInput,
  ElCheckboxGroup,
  ElCheckbox,
  ElRadioGroup,
  ElRadio,
  ElSelect
} from 'element-plus';
const props = defineProps({
  class: {
    type: String,
    default: ''
  },
  formData: {
    type: Object,
    default: () => ({})
  },
  config: {
    type: Array,
    default: () => []
  }
});
</script>

<template>
  <el-form
    ref="form"
    :class="['sy-dynamic-form', props.class]"
    :model="props.formData"
    v-bind="$attrs"
    v-on="$listeners"
  >
    <el-form-item v-for="(item, index) in props.config" :key="item.id + index" :prop="item.prop" v-bind="item.attrs">
      <!-- 具名插槽 -->
      <slot v-if="item.slot" :name="item.slot" :data="item" :row="props.formData"></slot>
      <el-cascader
        v-else-if="item.componentName === 'el-cascader'"
        :key="Date.now()"
        :is="item.componentName"
        :options="item.options"
        popper-class="sy-cascader"
        v-model="props.formData[item.id]"
        v-on="item.events"
        v-bind="item.componentProps"
      >
      </el-cascader>
      <component
        v-else
        :is="item.componentName"
        :options="item.options"
        v-model="props.formData[item.id]"
        v-on="item.events"
        v-bind="item.componentProps"
        @input="
          item.type === 'input' &&
            (props.formData[item.id] = props.formData[item.id].replace(/[^\w\u4E00-\u9FA5_]/g, ''))
        "
      >
        <template v-if="item.componentName === 'el-select'">
          <el-option
            v-for="(option, idx) in item.options"
            :label="option.label || option[item.keyValue.label]"
            :value="option.value || option[item.keyValue.value]"
            :key="idx"
          >
          </el-option>
        </template>
        <template v-if="item.componentName === 'el-checkbox-group'">
          <el-checkbox v-for="option in item.options" :label="option.label" :key="option.label">
            {{ option.text || option.label }}
          </el-checkbox>
        </template>
        <template v-if="item.componentName === 'el-radio-group'">
          <el-radio v-for="option in item.options" :label="option.label" :key="option.label">
            {{ option.text || option.label }}
          </el-radio>
        </template>
      </component>
    </el-form-item>
    <!-- slot 默认插槽保留 -->
    <slot name="extend" :row="props.formData"></slot>
  </el-form>
</template>

<style lang="less" scoped></style>
