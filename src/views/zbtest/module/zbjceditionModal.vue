<template>
  <j-modal
    :title="title"
    :width="width"
    :visible="visible"
    :confirmLoading="confirmLoading"
    switchFullscreen
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭"
  >
    <a-spin :spinning="confirmLoading">
      <a-form :form="form" >
        <a-form-item
          label="隶属上级单位"
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
        >
          <div class="form-item" style="border: none">
            {{ record.subordinateUnits }}
          </div>
        </a-form-item>
        <a-form-item
            label="抽查问题记录"
            :labelCol="labelCol"
            :wrapperCol="wrapperCol"
        >
          <div class="form-item" style="border: none">
            {{ record.subordinateUnits }}
          </div>
        </a-form-item>
        <a-form-item
          label="下发时间"
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
        >
          <div class="form-item" style="border: none">
            {{ record.startTime }}
          </div>
        </a-form-item>

        <a-form-item
          label="回执时间"
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
        >
          <div class="form-item" style="border: none">{{ record.endTime }}</div>
        </a-form-item>

        <a-form-item label="抽查过程记录" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-textarea
                  class="form-item"
                  style="margin-top:10px"
                  v-model="record.checkResult"
                  :auto-size="{ minRows: 8, maxRows: 20 }"
                  :maxLength="400"
              />
        </a-form-item>
        <a-form-item
          label="问题数量"
          :labelCol="labelCol"
          :wrapperCol="wrapperCol"
        >
          <a-input
            v-model="record.checkResultStatus"
            placeholder="请输入问题数量"
            tyle="width:100%"
            class="form-item"
          />
        </a-form-item>
      </a-form>
    </a-spin>
  </j-modal>
</template>

<script>
import { httpAction } from "@/api/manage";
import pick from "lodash.pick";
import JDictSelectTag from "@/components/dict/JDictSelectTag";

export default {
  name: "DictVueTestModal",
  components: {
    JDictSelectTag,
  },
  data() {
    return {
      form: this.$form.createForm(this),
      title: "操作",
      width: 600,
      visible: false,
      record: {},
      labelCol: {
        xs: { span: 8 },
        sm: { span: 8 },
      },
      wrapperCol: {
        xs: { span: 10 },
        sm: { span: 12 },
      },
      confirmLoading: false,
      validatorRules: {},
      url: {
        edit: "/southCheckTask/edit",
      },
    };
  },
  created() {},
  methods: {
    edit(record) {
      this.visible = true;
      this.record = record;
    },
    close() {
      this.$emit("close");
      this.record ={}
      this.visible = false;
    },
    handleOk() {
      const that = this;
      httpAction(this.url.edit, this.record, "put")
        .then((res) => {
          if (res.success) {
            that.$message.success(res.message);
            that.$emit("ok");
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
  },
};
</script>
<style scoped>
.form-item {
  margin-left: 15px;
}
</style>