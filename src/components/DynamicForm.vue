<script setup lang="ts">
import {
  ElForm,
  ElFormItem,
  ElOption,
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
  },
  labelPosition: {
    type: String as any,
    default: 'right'
  },
  labelWidth: {
    type: String as any,
    default: 'auto'
  }
});
defineOptions({
  name: 'DynamicForm',
  components: {
    ElForm,
    ElFormItem,
    ElOption,
    ElCascader,
    ElInput,
    ElCheckboxGroup,
    ElCheckbox,
    ElRadio,
    ElRadioGroup,
    ElSelect
  }
});
</script>

<template>
  <el-form
    ref="form"
    :class="['sy-dynamic-form', props.class]"
    :model="props.formData"
    :label-position="props.labelPosition"
    :label-width="props.labelWidth"
    v-bind="$attrs"
  >
    <el-form-item
      v-for="(item, index) in props.config"
      :key="(item as any).id + index"
      :prop="(item as any).field"
      :label="(item as any).label"
      v-bind="(item as any).attrs"
    >
      <!-- 具名插槽 -->
      <slot v-if="(item as any).slot" :name="(item as any).slot" :data="item" :row="props.formData"></slot>
      <el-cascader
        v-else-if="(item as any).componentName === 'ElCascader'"
        :key="Date.now()"
        :options="(item as any).options"
        popper-class="sy-cascader"
        v-model="props.formData[(item as any).id]"
        v-bind="(item as any).componentProps"
      >
      </el-cascader>
      <component
        v-else
        :is="(item as any).componentName"
        :options="(item as any).options"
        v-model="props.formData[(item as any).id]"
        v-bind="(item as any).componentProps"
        @input="
          (item as any).type === 'input' &&
            (props.formData[(item as any).id] = props.formData[(item as any).id].replace(/[^\w\u4E00-\u9FA5_]/g, ''))
        "
      >
        <template v-if="(item as any).componentName === 'ElSelect'">
          <el-option
            v-for="(option, idx) in (item as any).options"
            :label="option.label || option[(item as any).keyValue.label]"
            :value="option.value || option[(item as any).keyValue.value]"
            :key="idx"
          >
          </el-option>
        </template>
        <template v-if="(item as any).componentName === 'ElCheckboxGroup'">
          <el-checkbox v-for="option in (item as any).options" :label="option.label" :key="option.label">
            {{ option.text || option.label }}
          </el-checkbox>
        </template>
        <template v-if="(item as any).componentName === 'ElCheckboxGroup'">
          <el-radio v-for="option in (item as any).options" :label="option.label" :key="option.label">
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
