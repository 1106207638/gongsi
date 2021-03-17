<template>
  <j-modal
      :title="title"
      :width="width"
      :visible="visible"
      :fullscreen="false"
      :confirmLoading="confirmLoading"
      switchFullscreen
      @ok="handleOk"
      @cancel="handleCancel"
      cancelText="关闭">
    <a-spin :spinning="confirmLoading">

      <a-radio-group v-model="value">
        <a-radio :style="radioStyle" value="normal">
          普通同步(保留表数据)
        </a-radio>
        <a-radio :style="radioStyle" value="force">
          强制同步(删除表,重新生成)
        </a-radio>
      </a-radio-group>
    </a-spin>
  </j-modal>
</template>

<script>

import {getAction, httpAction} from '@/api/manage'
import pick from 'lodash.pick'
import {validateDuplicateValue} from '@/utils/util'
import JDictSelectTag from "@/components/dict/JDictSelectTag"


export default {
  name: "dataBase",
  components: {
    JDictSelectTag,
  },
  prop: {
    taskId: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      radioStyle: {
        display: 'block',
        height: '30px',
        lineHeight: '30px',
      },
      value:'normal',
      record:null,
      title: "操作",
      width: 500,
      visible: false,
      model: {},
      labelCol: {
        xs: {span: 24},
        sm: {span: 5},
      },
      wrapperCol: {
        xs: {span: 24},
        sm: {span: 16},
      },
      form: this.$form.createForm(this),
      confirmLoading: false,
      validatorRules: {},
      url: {
        // 同步数据库
        dataBase: "online/cgform/api/doDbSynch/",
      },
    }
  },
  created() {
  },
  methods: {
    edit(record) {
      console.log(record)
      this.record = record.id
      this.taskId =""
      this.form.resetFields();
      this.model = Object.assign({}, record);
      this.visible = true;
      this.$nextTick(() => {
        this.form.setFieldsValue(pick(this.model))
      })
    },
    close() {
      this.$emit('close');
      this.visible = false;
    },
    handleOk() {
      const that = this;
      // 触发表单验证
      that.confirmLoading = true;
      let httpurl = '';
      let method = '';
      if (!this.model.id) {
        httpurl += this.url.dataBase+this.record+'/'+this.value
        method = 'post';
      } else {
        httpurl += this.url.dataBase+this.record+'/'+this.value
        method = 'post';
      }
      let formData = {}
      httpAction(httpurl, formData, method).then((res) => {
        if (res.success) {
          that.$message.success(res.message);
          that.$emit('ok');
        } else {
          that.$message.warning(res.message);
        }
      }).finally(() => {
        that.confirmLoading = false;
        that.close();
      })


    },
    handleCancel() {
      this.close()
    },
    popupCallback(row) {
      this.form.setFieldsValue(pick(row, 'name'))
    },

  }
}
</script>
<style scoped>
.form-item {
  margin-left: 15px
}
/deep/ .form-item {
  margin-left: 0;
}

</style>