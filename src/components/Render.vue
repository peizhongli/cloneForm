<template>
  <el-form-item :label="formOptions.label" label-width="100px">
    <!-- 下拉框 -->
    <el-select v-if="formOptions.type==='select'" v-model="formOptions.value" placeholder="请选择">
      <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value"></el-option>
    </el-select>
    <!-- 单行文本框 -->
    <el-input v-if="formOptions.type==='input'" v-model="formOptions.value" placeholder="请输入内容"></el-input>
    <!-- 多行文本框 -->
    <el-input
      v-if="formOptions.type==='textarea'"
      type="textarea"
      :rows="2"
      placeholder="请输入内容"
      v-model="formOptions.value"
    ></el-input>
    <!-- 日期选择器 -->
    <el-date-picker v-if="formOptions.type==='datePicker'" v-model="formOptions.value" type="date" placeholder="选择日期"></el-date-picker>
    <!-- 按钮组 -->
    <section class="btn-group" v-if="customForm">
      <el-button type="primary" icon="el-icon-edit" plain circle size="mini"></el-button>
      <el-button type="danger" icon="el-icon-delete" plain circle size="mini"></el-button>
    </section>
  </el-form-item>
</template>
<script>
export default {
  name: "renders",
  props: {
    formOptions: Object,
    customForm: Boolean,
  },
  data() {
    return {
      options: [
        { label: "数据1", value: 1 },
        { label: "数据2", value: 2 },
        { label: "...", value: 3 }
      ]
    };
  }
};
</script>
<style scoped>
.btn-group {
    display: inline-block;
}
</style>