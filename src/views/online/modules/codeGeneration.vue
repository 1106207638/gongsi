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
      <a-form :form="form">
        <a-form-item label="代码生成目录" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-search  v-decorator="['projectPath',{}]" size="normal" @search="onSearch">
            <a-button slot="enterButton" icon="folder-open">
              浏览
            </a-button>
          </a-input-search>
        </a-form-item>
        <a-form-item label="页面分格" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-select  v-decorator="['jspMode']">
            <a-select-option v-for="(item,index) in jspModeList" :key="index" :value="item.code">
              {{ item.note }}
            </a-select-option>
          </a-select>
        </a-form-item>
        <a-form-item label="功能说明" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['ftlDescription',{}]"></a-input>

        </a-form-item>
        <a-form-item label="表名" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['tableName',{
                 rules: [{ required: true, message: '请输入表名' }]
              }]"></a-input>
        </a-form-item>
        <a-form-item label="实体类名" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['entityName',{
                 rules: [{ required: true, message: '请输入实体类名' }]
              }]"></a-input>
          <!--              <j-dict-select-tag type="list" v-decorator="['idType']" :trigger-change="true"-->
          <!--                                 dictCode="theme_Template" placeholder="请选择检查结果" class="form-item"/>-->

        </a-form-item>
        <a-form-item label="包名（小写）" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['entityPackage',{
                 rules: [{ required: true, message: '请输入包名' }]
              }]"></a-input>
        </a-form-item>
        <a-form-item label="代码分层样式" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <!--              <j-dict-select-tag type="list" v-decorator="['themeTemplate']" :trigger-change="true"-->
          <!--                                 dictCode="theme__Template" placeholder="请选择检查结果" class="form-item"/>-->
          <a-select v-decorator="['packageStyle',{}]" disabled >
            <a-select-option value="service">
              业务分层
            </a-select-option>
          </a-select>
        </a-form-item>

      </a-form>
      <catalog ref="catalog" @ok="toggleYES" />
      <result ref="result" @ok="toggleYES" />
    </a-spin>
  </j-modal>

</template>

<script>

import {getAction, httpAction} from '@/api/manage'
import pick from 'lodash.pick'
import {validateDuplicateValue} from '@/utils/util'
import JDictSelectTag from "@/components/dict/JDictSelectTag"
import catalog  from "@views/online/modules/catalog";
import result from "@views/online/modules/result";
export default {
  name: "codeGeneration",
  components: {
    JDictSelectTag,
    catalog,
    result
  },
  prop: {
    taskId: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      jspModeList:[],
      // 选中的行数
      code:"",
      selectedRowIds:[],
      form: this.$form.createForm(this),
      title: "操作",
      width: 800,
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
      confirmLoading: false,
      validatorRules: {},
      url: {
        form: "/online/cgform/head/tableInfo",
        sc:'/online/cgform/api/codeGenerate',
      },
    }
  },
  created() {
  },
  methods: {
    // 选择完成
    toggleYES(data) {
      this.form.validateFields((error, values) => {//获取form表单中的数据
        if (!error) {//当没有错误时调用提交方法
          let formData = values
          formData.projectPath = data[0]
          this.edit(formData)
        }
      })

    },
    onSearch() {
      this.$refs.catalog.edit()
      this.$refs.catalog.title = '选择目录'
    },
    add(code) {
      var httpurl = this.url.form
      var obj = {
        code: code
      }
      this.code = code
      getAction(httpurl, obj).then((res) => {
        console.log(res)
        if (res.success) {
          this.$message.success(res.message);
          var mulu = res.result.projectPath
          var xinxi = res.result.main
          var jsplist = res.result.jspModeList
          this.jspModeList = res.result.jspModeList
          var name = xinxi.tableName
          var str = ''
          for(var i = 0 ; i < name.length;i++) {
            if(i==0) {
              str+=name[i].toUpperCase()
            }else if(name[i]=='_') {
              str+=name[i]
              str+=name[i+1].toUpperCase()
              i++
            }else {
              str+=name[i]
            }
          }
          var record = {
            code: this.code,
            codeTypes:"controller,service,dao,mapper,entity,vue",
            entityName:str,
            entityPackage:'',
            ftlDescription:xinxi.tableTxt,
            jformType:'1',
            jspMode:'one',
            packageStyle:'service',
            projectPath: mulu,
            tableName:xinxi.tableName,
            tableName_tmp:xinxi.tableName
          }
          this.edit(record);
        } else {
          that.$message.warning(res.message);
        }
      }).finally(() => {
      })
      this.edit({

      });
    },
    edit(record) {
      record.taskId = this.taskId;
      this.taskId =""
      this.form.resetFields();
      this.model = Object.assign({}, record);
      this.visible = true;
      this.$nextTick(() => {
        this.form.setFieldsValue(pick(this.model,'code','codeTypes','entityName','entityPackage',
            'ftlDescription','jformType','jspMode','packageStyle','projectPath','tableName','tableName_tmp'))
      })
    },
    close() {
      this.$emit('close');
      this.visible = false;
    },
    handleOk() {
      const that = this;
      // 触发表单验证
      this.form.validateFields((err, values) => {
        if (!err) {
          that.confirmLoading = true;
          let httpurl = '';
          let method = '';
          if (!this.model.id) {
            httpurl += this.url.sc;
            method = 'post';
          } else {
            httpurl += this.url.sc;
            method = 'post';
          }
          let formData = Object.assign(this.model, values);
          console.log("表单提交数据", formData)

          httpAction(httpurl, formData, method).then((res) => {
            if (res.success) {
              that.$message.success(res.message);
              // that.$emit('ok');
              that.$refs.result.add(res.result)
            } else {
              that.$message.warning(res.message);
            }
          }).finally(() => {
            that.confirmLoading = false;
            that.close();
          })
        }

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