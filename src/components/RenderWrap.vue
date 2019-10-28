<template>
  <div>
    <el-card>
      <div slot="header" class="clearfix">
        <h3>{{mainSetting.title}}</h3>
      </div>
      <el-link
        icon="el-icon-setting"
        class="edit-btn"
        :underline="false"
        v-if="model==='custom'"
        @click="setMain"
      ></el-link>
      <draggable :list="list" :options="options" element="el-form">
        <renders v-for="element in list" :key="element.id" :element="element" :model="model"></renders>
      </draggable>
      <div class="dialog-btn-group">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="confirmOpt">确 定</el-button>
      </div>
    </el-card>
    <el-dialog title="弹窗参数设置" :visible.sync="dialogFormVisible">
      <el-checkbox-group v-model="mainSetting.checkList">
        <el-checkbox label="标题"></el-checkbox>
        <el-checkbox label="确认按钮"></el-checkbox>
        <el-checkbox label="取消按钮"></el-checkbox>
      </el-checkbox-group>
      <el-form :model="mainSetting" :label-width="'70px'" class="main-form">
        <el-form-item label="设置标题">
          <el-input v-model="mainSetting.title" :disabled="mainSetting.checkList.indexOf('标题')<0"></el-input>
        </el-form-item>
        <el-form-item label="确认话术">
          <el-input
            v-model="mainSetting.confirm"
            :disabled="mainSetting.checkList.indexOf('确认按钮')<0"
          ></el-input>
        </el-form-item>
        <el-form-item label="取消话术">
          <el-input
            v-model="mainSetting.cancel"
            :disabled="mainSetting.checkList.indexOf('取消按钮')<0"
          ></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="confirmOpt">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
import draggable from "vuedraggable";
import renders from "./Render";
export default {
  name: "renderWrap",
  components: { draggable, renders },
  props: {
    list: Array,
    options: Object,
    model: String
  },
  data() {
    return {
      dialogFormVisible: false,
      mainSetting: {
        checkList: [],
        title: "请填写以下内容",
        confirm: "确认",
        cancel: "取消"
      }
    };
  },
  methods: {
    setMain() {
      this.dialogFormVisible = true;
    },
    confirmOpt() {
      //   this.mainSetting.checkList.forEach(i => {
      //     if (i === "标题" && this.mainSetting.title !== "") {
      //     }
      //   });
    }
  }
};
</script>
<style scoped>
.el-card {
  padding: 0 18px 18px;
  position: relative;
}
.edit-btn {
  position: absolute;
  top: 22px;
  right: 20px;
  color: #888;
  font-size: 18px;
}
.main-form {
  margin-top: 30px;
}
h3 {
  margin: 0;
  color: #666;
  font-weight: 400;
}
.dialog-btn-group {
    text-align: center;
}
</style>