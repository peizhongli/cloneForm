<template>
  <div>
    <section class="normal">
      <h3>所有</h3>
      <render-wrap :list="normalList" :options="normalOpt"></render-wrap>
    </section>
    <section class="custom">
      <h3>预览</h3>
      <render-wrap :list="customList" :options="customOpt" model="custom"></render-wrap>
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
        { id: 4, label: "日期选择器", type: "datePicker", value: "" },
      ]
    };
  },
  computed: {},
  methods: {
    handleDel(index, id) {
      this.list1.splice(index, 1);
      let q = this.list2.find(value => {
        return value.id === id;
      });
      this.$set(q, "flag", false);
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