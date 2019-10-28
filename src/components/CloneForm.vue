<template>
  <div>
    <section class="normal">
      <h3>所有</h3>
      <el-card>
        <draggable :list="normalList" :options="normalOpt" element="el-form">
          <renders v-for="element in normalList" :key="element.id" :formOptions="element"></renders>
        </draggable>
      </el-card>
    </section>
    <section class="custom">
      <h3>预览</h3>
      <el-card>
        <draggable :list="customList" :options="customOpt" element="el-form" style="height:300px;">
          <renders v-for="element in customList" :key="element.id" :formOptions="element" :customForm="true"></renders>
          <!-- <div v-for="(element, index) in list1" :key="element.id" class="list-complete-item">
            <div class="list-complete-item-handle">{{element.name}}</div>
            <div>
              <i class="el-icon-delete" @click="handleDel(index, element.id)"></i>
            </div>
          </div>-->
        </draggable>
      </el-card>
    </section>
  </div>
</template>
<script>
import draggable from "vuedraggable";
import renders from "./Render";
export default {
  name: "cloneForm",
  components: { draggable, renders },
  watch: {},
  data() {
    return {
      normalOpt: {
        group: { name: "form", pull: "clone", put: false },
        sort: false
      },
      customOpt: {
        group: "form",
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
.el-card {
  padding: 18px;
}
.custom,
.normal {
  width: 45%;
  padding: 0 15px;
  display: inline-block;
  vertical-align: top;
}
</style>