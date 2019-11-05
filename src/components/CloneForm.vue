<template>
  <div>
    <section class="normal">
      <h3>可配置项</h3>
      <render-wrap :list="normalList"></render-wrap>
    </section>
    <section class="custom">
      <h3>预览 <span class="el-icon-view" @click="showCard"></span></h3>
      <render-wrap
        :list="customList"
        model="custom"
        @deleteElement="deleteEle"
        @editElement="editEle"
      ></render-wrap>
    </section>
    <el-dialog :visible.sync="dialogVisible">
      <render-wrap
        :list="showList"
        model="preview"
        @deleteElement="deleteEle"
        @editElement="editEle"
      ></render-wrap>
    </el-dialog>
  </div>
</template>
<script>
import renderWrap from "./RenderWrap";
export default {
  name: "cloneForm",
  components: { renderWrap },
  watch: {},
  data() {
    return {
      customList: [],
      normalList: [
        { id: 1, label: "输入框", type: "input", value: "" },
        { id: 2, label: "下拉框", type: "select", value: "" },
        { id: 3, label: "多行文本框", type: "textarea", value: "" },
        { id: 4, label: "日期选择器", type: "datePicker", value: "" }
      ],
      dialogVisible: false,
      showList: [],
    };
  },
  computed: {},
  methods: {
    deleteEle(ele, index) {
      this.customList.splice(index, 1);
    },
    editEle(ele, index) {
      for (let i = 0; i < this.customList.length; i++) {
        if (i === index) {
          for(let j in ele) {
            this.customList[i][j]=ele[j]
          }
          console.log(this.customList[i]);
          break;
        }
      }
    },
    showCard() {
      this.dialogVisible = true;
    }
  }
};
</script>
<style scoped>
.custom,
.normal {
  width: 45%;
  padding: 0 15px;
  display: inline-block;
  vertical-align: top;
}
</style>