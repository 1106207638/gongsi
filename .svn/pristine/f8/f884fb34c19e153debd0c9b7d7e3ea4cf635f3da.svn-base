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
    cancelText="关闭">
    <a-spin :spinning="confirmLoading">
      <a-form :form="form">
<!--        <a-form-item label="父机构id" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
<!--          <j-tree-select-->
<!--            ref="treeSelect"-->
<!--            placeholder="请选择父机构id"-->
<!--            v-decorator="['pdataId']"-->
<!--            dict="south_data_classify_copy,data_type_name,id"-->
<!--            pidField="pdata_id"-->
<!--            pidValue="0"-->
<!--            hasChildField="has_child"-->
<!--            >-->
<!--          </j-tree-select>-->
<!--        </a-form-item>-->
        <a-form-item label="选择标签" style="width:100%" :labelCol="labelCol" v-if="addFORM" :wrapperCol="wrapperCol">
          <!-- <j-dict-select-tag type="list" v-decorator="['extractDemo']"
                             placeholder="请选择提取模板"/> -->
<!--          <a-select v-decorator="['extractDemo']" style="width: 100%" placeholder="请选择提取模板" @change="handleChange">-->
<!--            <a-select-option :value="item.id" v-for="(item,index) in dataList" :key="index">-->
<!--              {{ item.extractDocTitle }}-->
<!--            </a-select-option>-->
<!--          </a-select>-->
          <j-share-file ref="relation2"   :listUrl="url.flag"  v-decorator="['dataTypeName']" placeholder="标签" @change="shareChange"
                        :multiple="true"
          ></j-share-file>
        </a-form-item>
        <a-form-item label="标签分类" v-if="!addFORM" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['dataTypeName']" placeholder="请输入标签分类" ></a-input>
        </a-form-item>
        <a-form-item label="描述" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['description']" placeholder="请输入描述" ></a-input>
        </a-form-item>
        <a-form-item label="备注" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="['memo']" placeholder="请输入备注" ></a-input>
        </a-form-item>

      </a-form>
    </a-spin>
  </j-modal>
</template>

<script>

  import { httpAction } from '@/api/manage'
  import {dataGet} from '@/api/api'
  import pick from 'lodash.pick'
  import { validateDuplicateValue } from '@/utils/util'
  import JTreeSelect from '@/components/jeecg/JTreeSelect'
  import JShareFile from '@/components/jeecgbiz/JeecgshareFile'

  export default {
    name: "SouthDataClassifyModal",
    components: { 
      JTreeSelect,
      JShareFile
    },
    data () {
      return {
        form: this.$form.createForm(this),
        title:"操作",
        width:800,
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

        confirmLoading: false,
        validatorRules: {
        },
        url: {
          add: "southDataClassify/add",
          edit: "southDataClassify/edit",
          flag:'/online/cgreport/head/list'
        },
        expandedRowKeys:[],
        pidField:"pdataId",
        dataList:[],
        // 标签选择的数组
        selectFlagId:[],
        addFORM:false,
        isaddchild:false,
      }
    },
    created () {
    },
    methods: {
      shareChange(data) {
        this.selectFlagId = data
        console.log(this.selectFlagId)
      },
      // 获取标签
      getData() {
        dataGet({
          pageNo: 1,
          pageSize: 100
        }).then((res) => {
          if (res.success) {
            this.dataList = res.result.records
          }
        })
      },
      handleChange() {
      },
      add () {
        this.edit1({});
        this.isaddchild = false

      },
      add1 (record) {
        this.edit1(record);
        this.isaddchild = true
      },
      edit1 (record) {
        console.log(record)
        if(record.id) {
          this.addFORM = true
        }else {
          this.addFORM = false
        }
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,'pdataId','dataTypeName','description','memo'))
        })
      },
      edit (record) {
        this.isaddchild = false
        this.addFORM = false
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,'pdataId','dataTypeName','description','memo'))
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
          if (!err) {
            that.confirmLoading = true;
            let httpurl = '';
            let method = '';
            console.log(this.model)
            if(this.model.id&&this.isaddchild) {
              httpurl+=this.url.add;
              method = 'post';
            }else if(!this.model.id){
              httpurl+=this.url.add;
              method = 'post';
            }else{
              httpurl+=this.url.edit;
               method = 'put';
            }

            let old_pid = this.model[this.pidField]
            let formData = Object.assign(this.model, values);
            let new_pid = this.model[this.pidField]
            console.log("表单提交数据",formData)
            httpAction(httpurl,formData,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                this.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
              that.close();
            })
          }
         
        })
      },
      handleCancel () {
        this.close()
      },
      popupCallback(row){
        this.form.setFieldsValue(pick(row,'pdataId','dataTypeName','description','memo'))
      },
      submitSuccess(formData,flag){
        if(!formData.id){
          let treeData = this.$refs.treeSelect.getCurrTreeData()
          this.expandedRowKeys=[]
          this.getExpandKeysByPid(formData[this.pidField],treeData,treeData)
          this.$emit('ok',formData,this.expandedRowKeys.reverse());
        }else{
          this.$emit('ok',formData,flag);
        }
      },
      getExpandKeysByPid(pid,arr,all){
        if(pid && arr && arr.length>0){
          for(let i=0;i<arr.length;i++){
            if(arr[i].key==pid){
              this.expandedRowKeys.push(arr[i].key)
              this.getExpandKeysByPid(arr[i]['parentId'],all,all)
            }else{
              this.getExpandKeysByPid(pid,arr[i].children,all)
            }
          }
        }
      }
      
      
    }
  }
</script>