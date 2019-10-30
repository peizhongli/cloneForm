<template>
  <el-form-item
    :label="element.label"
    label-width="100px"
    :class="model==='custom'?'custom-item':''"
  >
    <!-- 下拉框 -->
    <el-select v-if="element.type==='select'" v-model="element.value" placeholder="请选择">
      <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value"></el-option>
    </el-select>
    <!-- 单行文本框 -->
    <el-input v-if="element.type==='input'" v-model="element.value" placeholder="请输入内容"></el-input>
    <!-- 多行文本框 -->
    <el-input
      v-if="element.type==='textarea'"
      type="textarea"
      :rows="2"
      placeholder="请输入内容"
      v-model="element.value"
    ></el-input>
    <!-- 日期选择器 -->
    <el-date-picker
      v-if="element.type==='datePicker'"
      v-model="element.value"
      type="date"
      placeholder="选择日期"
    ></el-date-picker>
    <!-- 按钮组 -->
    <section class="btn-group" v-if="model==='custom'">
      <el-button
        type="primary"
        icon="el-icon-edit"
        plain
        circle
        size="mini"
        @click="editElement(element)"
      ></el-button>
      <el-button
        type="danger"
        icon="el-icon-delete"
        plain
        circle
        size="mini"
        @click="deleteElement(element)"
      ></el-button>
    </section>
  </el-form-item>
</template>
<script>
export default {
  name: "renders",
  props: {
    element: Object,
    model: String
  },
  data() {
    return {
      options: [
        { label: "数据1", value: 1 },
        { label: "数据2", value: 2 },
        { label: "...", value: 3 }
      ]
    };
  },
  methods: {
    editElement(ele) {
      this.$emit("editElement", ele);
    },
    deleteElement(ele) {
      this.$emit("deleteElement", ele);
    }
  }
};
</script>
<style scoped>
.btn-group {
  display: inline-block;
  vertical-align: top;
  margin-left: 10px;
}
.custom-item >>> .el-form-item__content > div {
  max-width: 80%;
}
</style>