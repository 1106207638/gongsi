<template>
  <j-modal
    :title="title"
    :width="width"
    :visible="visible"
    :confirmLoading="confirmLoading"
    switchFullscreen
    @ok="handleOk"
    @cancel="handleCancel"
    :destroyOnClose="true"
    cancelText="关闭"
  >
    <a-spin :spinning="confirmLoading">
      <a-form :form="form">
        <a-form-item
          label="选择模板"
          style="width: 100%"
          :labelCol="labelCol"
          v-if="addFORM"
          :wrapperCol="wrapperCol"
        >
          <j-share-file
            ref="relation2"
            :listUrl="url.templateUrl"
            v-decorator="['dataTypeName']"
            placeholder="标签"
            @change="shareChange"
            :multiple="true"
          ></j-share-file>
        </a-form-item>
      </a-form>
    </a-spin>
  </j-modal>
</template>

<script>
import { httpAction } from "@/api/manage";
import { dataGet } from "@/api/api";
import pick from "lodash.pick";
import { validateDuplicateValue } from "@/utils/util";
import JTreeSelect from "@/components/jeecg/JTreeSelect";
import JShareFile from "@/components/jeecgbiz/JeecgshareFile1";

export default {
  name: "SouthDataClassifyModal",
  components: {
    JTreeSelect,
    JShareFile,
  },
  data() {
    return {
      form: this.$form.createForm(this),
      title: "操作",
      width: 800,
      visible: false,
      model: {},
      labelCol: {
        xs: { span: 24 },
        sm: { span: 5 },
      },
      wrapperCol: {
        xs: { span: 24 },
        sm: { span: 16 },
      },
      id: "",
      confirmLoading: false,
      validatorRules: {},
      url: {
        add: "southDataClassify/add",
        edit: "southDataClassify/edit",
        templateUrl: "/reportFormModel/queryAllModel1",
        toggleModuleUrl: "/reportFormModel/putSeatModel",
      },
      expandedRowKeys: [],
      pidField: "pdataId",
      dataList: [],
      // 标签选择的数组
      selectFlagId: [],
      addFORM: false,
      isaddchild: false,
    };
  },
  created() {},
  methods: {
    shareChange(data) {
      this.selectFlagId = data;
    },
    // 获取标签
    getData() {
      dataGet({
        pageNo: 1,
        pageSize: 100,
      }).then((res) => {
        if (res.success) {
          this.dataList = res.result.records;
        }
      });
    },
    handleChange() {},
    add() {
      this.edit1({});
      this.isaddchild = false;
    },
    add1(record) {
      this.edit1(record);
      this.isaddchild = true;
    },
    edit(record) {
      console.log(record);
      var arr = [];
      for (var i = 0; i < record.data.length; i++) {
        arr.push(record.data[i].headName);
      }
      var dataTypeName = arr.join(",");
      record.dataTypeName = dataTypeName;
      if (record.id) {
        this.addFORM = true;
        this.id = record.id;
      } else {
        this.addFORM = false;
        this.id = "";
      }
      this.form.resetFields();
      this.model = Object.assign({}, record);
      this.visible = true;
      this.$nextTick(() => {
        this.form.setFieldsValue(
          pick(this.model, "pdataId", "dataTypeName", "description", "memo")
        );
      });
    },
    close() {
      this.$emit("close");
      this.visible = false;
    },
    handleOk() {
      const that = this;
      // 触发表单验证
      that.confirmLoading = true;
      let httpurl = this.url.toggleModuleUrl;
      let method = "put";
      let formData = {
        ids: this.selectFlagId,
        id: this.id,
      };
      httpAction(httpurl, formData, method)
        .then((res) => {
          if (res.success) {
            that.$message.success(res.message);
            this.$emit("ok");
          } else {
            that.$message.warning(res.message);
          }
        })
        .finally(() => {
          that.confirmLoading = false;
          that.close();
        });
    },
    handleCancel() {
      this.close();
    },
    popupCallback(row) {
      this.form.setFieldsValue(
        pick(row, "pdataId", "dataTypeName", "description", "memo")
      );
    },
    submitSuccess(formData, flag) {
      if (!formData.id) {
        let treeData = this.$refs.treeSelect.getCurrTreeData();
        this.expandedRowKeys = [];
        this.getExpandKeysByPid(formData[this.pidField], treeData, treeData);
        this.$emit("ok", formData, this.expandedRowKeys.reverse());
      } else {
        this.$emit("ok", formData, flag);
      }
    },
    getExpandKeysByPid(pid, arr, all) {
      if (pid && arr && arr.length > 0) {
        for (let i = 0; i < arr.length; i++) {
          if (arr[i].key == pid) {
            this.expandedRowKeys.push(arr[i].key);
            this.getExpandKeysByPid(arr[i]["parentId"], all, all);
          } else {
            this.getExpandKeysByPid(pid, arr[i].children, all);
          }
        }
      }
    },
  },
};
</script>