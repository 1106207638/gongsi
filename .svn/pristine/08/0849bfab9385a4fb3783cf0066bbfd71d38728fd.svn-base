<template>
  <j-modal
    :title="title"
    :width="width"
    :visible="visible"
    :confirmLoading="confirmLoading"
    switchFullscreen
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭">
    <a-spin :spinning="confirmLoading">
      <a-form :form="form" >

        <a-form-item label="隶属上级单位" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <a-input  v-decorator="['subordinateUnits']" tyle="width:100%"  :disabled="true" />
        </a-form-item>

        <a-form-item :label="time1" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <j-date :placeholder="'请选择'+time1" v-decorator="['startTime', { rules: [{ required: true, message: this.time1+'为必填项!' }] }]"  style="width:100%"   :showTime="true" dateFormat="YYYY-MM-DD HH:mm:ss" />
        </a-form-item>

        <a-form-item label="接通时间" :labelCol="labelCol" :wrapperCol="wrapperCol" v-if="checkWay =='View'">
            <j-date placeholder="请选择接通时间" v-decorator="['responseTime', { rules: [{ required: true, message:'接通时间为必填项!' }] }]"  style="width:100%"  :showTime="true" dateFormat="YYYY-MM-DD HH:mm:ss" />
        </a-form-item>

        <a-form-item :label="time2" :labelCol="labelCol" :wrapperCol="wrapperCol">
            <j-date :placeholder="'请选择'+time2" v-decorator="['endTime', { rules: [{ required: true, message:this.time2+'为必填项!' }] }]"  style="width:100%"   :showTime="true" dateFormat="YYYY-MM-DD HH:mm:ss"  />
        </a-form-item>

        <a-form-item label="检查模板" :labelCol="labelCol" :wrapperCol="wrapperCol" v-if="checkWay == 'OperationalData'">
            <a-select   placeholder="请选择作战数据模板">
               <a-select-option value="jack">模板一</a-select-option>
                <a-select-option value="lucy"> 模板二</a-select-option>
                <a-select-option value="Yiminghe"> 模板三 </a-select-option>
            </a-select>
        </a-form-item>

        <a-form-item label="上传文件" :labelCol="labelCol" :wrapperCol="wrapperCol" v-if="checkWay == 'OperationalData'">
           <div >
              <j-upload v-model="fileList" :number="1">
                <div>aaaaaaaaaaaaaaaa</div>
              </j-upload>
              <j-ellipsis :value="fileList" :length="30" v-if="fileList.length>0"/>
           </div>
        </a-form-item>
        <a-form-item label="数据对比选择" :labelCol="labelCol" :wrapperCol="wrapperCol" v-if="checkWay == 'OperationalData'">
           <div class="form-item">
              <a-row>
                <a-col :span="12">
                  <a-checkbox @change="onChange">211K数据 </a-checkbox> 
                </a-col>
              </a-row>

              <a-row>
                <a-col :span="12">
                  <a-checkbox @change="onChange">周实力数据 </a-checkbox>
                </a-col>

                <a-col :span="12">
                  <a-select placeholder="请选择周实力表"  >
                    <a-select-option value="jack">XXX周实力表</a-select-option>
                    <a-select-option value="lucy"> XXX周实力表</a-select-option>
                    <a-select-option value="Yiminghe"> XXX周实力表 </a-select-option>
                  </a-select>                
                </a-col>
              </a-row>

              <a-row>
                <a-col :span="6">
                  <a-button type="primary"  @click="searchQuery" >对比 </a-button>
                </a-col>
              </a-row>
           </div>
        </a-form-item>   

        <a-form-item label="抽查过程记录" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-textarea
              v-decorator="['checkResult']"
              :auto-size="{ minRows: 8, maxRows: 20 }"
              :maxLength="400"
          />
        </a-form-item>

        <a-form-item label="问题数量" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input  v-decorator="['checkResultStatus']" placeholder="请输入问题数量" tyle="width:100%" />
        </a-form-item>
        
      </a-form>
    </a-spin>
  </j-modal>
</template>

<script>

  import { httpAction } from '@/api/manage'
  import pick from 'lodash.pick'
  import JDictSelectTag from "@/components/dict/JDictSelectTag"
  import JDate from '@/components/jeecg/JDate'  


  export default {
    name: "DictVueTestModal",
    components: { 
      JDictSelectTag,
      JDate
    },
  
    data () {
      return {
        form: this.$form.createForm(this),
        title:"操作",
        width:800,
        visible: false,
        model: {},
        labelCol: {
          xs: { span: 18 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        confirmLoading: false,
        url: {
          execute: "/southCheckTask/executeCheckTask",
        },
        taskId: "",
        checkWay:"",
        time1:"",
        time2:"",
        fileList:[]
      }
    },
    created () {
    },
    methods: {
      add () {
        this.edit({});
      },
      edit (record) {
        if(record.checkResultStatus ==0){
          record.checkResultStatus = null
        }
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;
        this.$nextTick(() => {
            this.form.setFieldsValue(pick(this.model,'subordinateUnits','startTime','responseTime','endTime','checkResult','checkResultStatus'))
        })
      },
      close () {
        this.$emit('close');
        this.visible = false;
      },
      handleOk () {
        const that = this;
        // 触发表单验证
        this.form.validateFields((err, values) => {
          values.taskId = this.taskId;
          if (!err) {
            that.confirmLoading = true;
            let formData = Object.assign(this.model, values);
            //文件路径
            formData.checkResultFile = this.fileList[0]
            console.log("表单提交数据",formData)
            httpAction(this.url.execute,formData,'post').then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                that.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
              that.model={}
              that.fileList = []
              that.close();
            })
          }
         
        })
      },
      handleCancel () {
        this.close()
      },
      popupCallback(row){
        this.form.setFieldsValue(pick(row,'name'))
      },

    }
  }
</script>
<style scoped>
.form-item{
  margin-left:15px
}
</style>