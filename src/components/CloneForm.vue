<template>
  <div>
    <section class="normal">
      <h3>所有</h3>
      <render-wrap :list="normalList" :options="normalOpt"></render-wrap>
    </section>
    <section class="custom">
      <h3>预览</h3>
      <render-wrap
        :list="customList"
        :options="customOpt"
        model="custom"
        @deleteElement="deleteEle"
        @editElement="editEle"
      ></render-wrap>
    </section>
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
      normalOpt: {
        group: { name: "form", pull: "clone", put: false },
        sort: false
      },
      customOpt: {
        group: "form"
      },
      customList: [],
      normalList: [
        { id: 1, label: "输入框", type: "input", value: "" },
        { id: 2, label: "下拉框", type: "select", value: "" },
        { id: 3, label: "多行文本框", type: "textarea", value: "" },
        { id: 4, label: "日期选择器", type: "datePicker", value: "" }
      ]
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