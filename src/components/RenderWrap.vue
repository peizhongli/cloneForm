<template>
  <div id="renderWrap">
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
      <draggable :list="list" :options="options" element="el-form" class="main-wrap">
        <renders
          v-for="(element,index) in list"
          :key="element.id"
          :element="element"
          :model="model"
          @editElement="editElement(element,index)"
          @deleteElement="deleteElement(element,index)"
        ></renders>
      </draggable>
      <div class="dialog-btn-group">
        <el-button @click="dialogFormVisible = false">{{mainSetting.cancel}}</el-button>
        <el-button type="primary" @click="confirmOpt">{{mainSetting.confirm}}</el-button>
      </div>
    </el-card>
    <el-dialog :title="dialogTitle" :visible.sync="dialogFormVisible" top="0">
      <section class="form-content" v-if="dialogTitle==='弹窗参数设置'">
        <el-form :model="mainSetting" :label-width="'70px'" class="main-form">
          <el-form-item label="显示参数">
            <el-checkbox-group v-model="mainSetting.checkList">
              <el-checkbox label="标题"></el-checkbox>
              <el-checkbox label="确认按钮"></el-checkbox>
              <el-checkbox label="取消按钮"></el-checkbox>
            </el-checkbox-group>
          </el-form-item>
          <el-form-item label="设置标题">
            <el-input
              v-model="mainSetting.newTitle"
              :disabled="mainSetting.checkList.indexOf('标题')<0"
            ></el-input>
          </el-form-item>
          <el-form-item label="确认话术">
            <el-input
              v-model="mainSetting.newConfirm"
              :disabled="mainSetting.checkList.indexOf('确认按钮')<0"
            ></el-input>
          </el-form-item>
          <el-form-item label="取消话术">
            <el-input
              v-model="mainSetting.newCancel"
              :disabled="mainSetting.checkList.indexOf('取消按钮')<0"
            ></el-input>
          </el-form-item>
        </el-form>
      </section>
      <section class="element-content" v-if="dialogTitle==='元素参数设置'">
        <el-form :model="eleSetting" :label-width="'90px'" class="main-form">
          <el-form-item label="元素名称">
            <el-input v-model="eleSetting.label"></el-input>
          </el-form-item>
          <el-form-item label="唯一标识" required>
            <el-input v-model="eleSetting.dictionary"></el-input>
          </el-form-item>
          <el-form-item label="占位内容">
            <el-input v-model="eleSetting.placeholder"></el-input>
          </el-form-item>
          <el-form-item label="最大长度">
            <el-input v-model="eleSetting.maxLength"></el-input>
          </el-form-item>
          <el-form-item label="输入类型" v-show="eleSetting.type==='input'">
            <el-radio-group v-model="eleSetting.contentType">
              <el-radio label="text">文本</el-radio>
              <el-radio label="number">数字</el-radio>
              <el-radio label="password">密码</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="是否必填">
            <el-switch v-model="eleSetting.required"></el-switch>
          </el-form-item>
          <el-form-item label="是否可清空">
            <el-switch v-model="eleSetting.clearable"></el-switch>
          </el-form-item>
        </el-form>
      </section>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="confirmOpt">确 认</el-button>
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
      dialogTitle: "弹窗参数设置",
      dialogFormVisible: false,
      mainSetting: {
        checkList: [],
        title: "请填写以下内容",
        confirm: "确认",
        cancel: "取消",
        newTitle: "请填写以下内容",
        newConfirm: "确认",
        newCancel: "取消"
      },
      eleSetting: {},
      editIndex: 0
    };
  },
  methods: {
    setMain() {
      this.dialogTitle = "弹窗参数设置";
      this.dialogFormVisible = true;
    },
    confirmOpt() {
      if (this.dialogTitle === "弹窗参数设置") {
        this.mainSetting.checkList.forEach(i => {
          if (i === "标题" && this.mainSetting.newTitle !== "") {
            this.mainSetting.title = this.mainSetting.newTitle;
          } else if (i === "确认按钮" && this.mainSetting.newConfirm !== "") {
            this.mainSetting.confirm = this.mainSetting.newConfirm;
          } else if (i === "取消按钮" && this.mainSetting.newCancel !== "") {
            this.mainSetting.cancel = this.mainSetting.newCancel;
          }
        });
      } else {
        this.$emit("editElement", this.eleSetting, this.editIndex);
      }

      this.dialogFormVisible = false;
    },
    editElement(ele, index) {
      this.dialogTitle = "元素参数设置";
      this.dialogFormVisible = true;
      for (let i = 0; i < this.eleSettingList.length; i++) {
        if (i === index) {
          this.eleSetting = JSON.parse(JSON.stringify(this.eleSettingList[i]));
          break;
        }
      }
      this.editIndex = index;
    },
    deleteElement(ele, index) {
      this.$emit("deleteElement", ele, index);
    }
  },
  computed: {
    eleSettingList() {
      let settingList = this.list.map(i => {
        return {
          label: i.label,
          required: false,
          placeholder: "请输入内容",
          maxLength: 20,
          dictionary: "",
          clearable: false,
          contentType: "text",
        };
      });
      return settingList;
    }
  }
};
</script>
<style scoped>
section {
  margin: 0;
  padding: 0;
}
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
h3 {
  margin: 0;
  color: #666;
  font-weight: 400;
}
.dialog-btn-group {
  text-align: center;
}
.main-wrap {
  padding-bottom: 15px;
}
#renderWrap >>> .el-dialog__body {
  padding: 30px 20px 0;
}
#renderWrap >>> .el-dialog {
  position: relative;
  margin: 0 auto;
  top: 50%;
  transform: translateY(-50%);
}
</style>