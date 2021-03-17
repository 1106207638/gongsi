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
      <a-directory-tree multiple default-expand-all :tree-data="treeData"
                        :load-data="onLoadData" @select="onSelect" @expand="onExpand">

      </a-directory-tree>
    </a-spin>
  </j-modal>
</template>

<script>

import {getAction, httpAction} from '@/api/manage'
import pick from 'lodash.pick'
import {validateDuplicateValue} from '@/utils/util'
import JDictSelectTag from "@/components/dict/JDictSelectTag"


export default {
  name: "catalog",
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
      treeData:[],
      pf:null,
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
        // 获取盘符
        getpf: "online/cgform/head/rootFile",
        getup:'online/cgform/head/fileTree'
      },
    }
  },
  created() {
  },
  methods: {
    onLoadData(treeNode) {
      return new Promise(resolve => {
        if (treeNode.dataRef.children) {
          resolve();
          return;
        }
        var obj = {
          parentPath:treeNode.dataRef.key
        }
        getAction(this.url.getup, obj).then((res) => {
          if (res.success) {
            treeNode.dataRef.children = res.result
            this.treeData = [...this.treeData];
            resolve();
          } else {
            this.$message.warning(res.message);
          }
        }).finally(() => {
        })
      });
    },
    onSelect(keys, event) {
      console.log('Trigger Select', keys, event);
      this.pf = keys
    },
    onExpand() {
    },
    edit(record) {
      this.visible = true;
      var url = this.url.getpf
      getAction(url, {}).then((res) => {
        console.log(res)
        if (res.success) {
          this.treeData = res.result
        } else {
          this.$message.warning(res.message);
        }
      }).finally(() => {
      })
    },
    close() {
      this.$emit('close');
      this.visible = false;
    },
    handleOk() {
      const that = this;
      if(that.pf) {
        that.$emit('ok',that.pf);
        that.visible = false;
      }else {
        that.$message.warning('请选择目录！');
      }
      // 触发表单验证


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