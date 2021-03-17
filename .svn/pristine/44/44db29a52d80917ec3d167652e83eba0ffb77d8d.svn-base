<template>
  <a-card :bordered="false" v-if="columns">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <a-form-item label="席位">
              <!-- <j-input placeholder="请输入名称模糊查询" v-model="queryParam.name"></j-input> -->
              {{ userInfo.departName }}
            </a-form-item>
          </a-col>
          <a-col :xl="3" :lg="6" :md="8" :sm="24">
            <a-form-item label="报告人">
              <!-- <a-input placeholder="请输入名称查询" v-model="queryParam.age"></a-input>-->
              <!-- <a-input placeholder="最小年龄" type="ge" v-model="queryParam.age_begin" style="width:calc(50% - 15px);"></a-input>
              <span class="group-query-strig">~</span>
              <a-input placeholder="最大年龄" type="le" v-model="queryParam.age_end" style="width:calc(50% - 15px);"></a-input> -->
              {{ userInfo.realName }}
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="6" :md="8" :sm="24">
            <a-form-item label="报告日期">
              <a-date-picker v-model="formData.createTime"
                             :disabled="dateDisable"
                             format="YYYY-MM-DD"
                             @change="onChange"
              />
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="6" :md="8" :sm="24">
            <a-form-item label="采报类型">
              <j-dict-select-tag @input="changeType" v-model="formData.type" :moren="false" placeholder="请选择采报类型"
                                 dictCode="ReportType"/>

            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="6" :md="8" :sm="24">
            <a-form-item label="采报模型">
              <a-select @change="templateChange" v-model="formData.template" :disabled="istype"
                        placeholder="请选择采报模型">
                <a-select-option :value="item.headId" :key="index" v-for="(item,index) in templateList">
                  {{ item.headName }}
                </a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <!-- <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
            <a-col :xl="4" :lg="6" :md="8" :sm="24">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
            </a-col>
          </span> -->
        </a-row>
      </a-form>
    </div>
    <j-demo ref="zxcb" v-if="jdemoshow" :formData="formData" :columns="columns" :importBtn="importBtn"
            :dataSource="dataSource"></j-demo>
    <a-tabs default-active-key="0" @change="callback" >
      <a-tab-pane :key="index" :tab="item.name" v-for="(item,index) in columnss" :forceRender="true" v-if="!jdemoshow">
        <j-demo :columns="item.columns"  :importBtn="true" :ref="'ref'+index" @loadDataUpload="loadDataUpload"
                @getDatas="getDatas" :formData="formData" :dataSource="item.dataSource"></j-demo>
      </a-tab-pane>
    </a-tabs>
  </a-card>
</template>

<script>
import {
  initDictOptions,
  filterDictText,
  filterDictTextByCache
} from '@/components/dict/JDictSelectUtil'
import {JeecgListMixin} from '@/mixins/JeecgListMixin'
import Vue from 'vue'
import {filterObj} from '@/utils/util';
import JDemo from './JDemo.vue';
import UploadDemo from './uploadDemo.vue';
import {getAction,postAction} from "@api/manage";
import {JVXETypes} from "@comp/jeecg/JVxeTable";
import {JEditableTableMixin} from '@/mixins/JEditableTableMixin'
import {FormTypes, getRefPromise} from '@/utils/JEditableTableUtil'

export default {
  name: "JeecgDemoList",
  mixins: [JeecgListMixin, JEditableTableMixin],
  components: {
    JDemo,
    UploadDemo
  },
  watch: {
    "columnss": {
      handler: function (val, oldVal) {
        this.columnss = val
      },
      deep: true
    }
  },
  data() {
    return {
      importBtn:false,
      disableMixinCreated: true,
      // 模板列表
      jdemoshow: false,
      templateList: [],
      istype: true,
      // 选择区域
      formData: {
        createTime: '',//时间
        type: 'SynthesizeLogging',//采报类型
        template: ''//模板
      },

      typeoption: [{
        name: '综合日志',
        value: 0
      }, {
        name: '执勤数据',
        value: 1
      }, {
        name: '执勤报文',
        value: 2
      }, {
        name: '军委填报表',
        value: 3
      }, {
        name: '军委系统表',
        value: 4
      }],
      stypeoption: [{
        name: '模型一',
        value: 0
      }, {
        name: '模型二',
        value: 1
      }, {
        name: '模型三',
        value: 2
      }, {
        name: '模型四',
        value: 3
      }, {
        name: '模型五',
        value: 4
      }],
      //列定义
      url: {
        list: "/southTable/queryTableData", //数据
        structure: '/southTable/queryTableStructure',//结构
        delete: "/test/jeecgDemo/delete",
        deleteBatch: "/test/jeecgDemo/deleteBatch",
        exportXlsUrl: "/test/jeecgDemo/exportXls",
        templateUrl: '/reportFormModel/queryAllModel',
        userInfo: '/southTable/queryUserInfo',//基本信息
        save: '/southTable/reportFromDataSave'
      },
      isshowlist: true,
      // 数据源，控制表格的数据
      dataSource: [],
      // 列配置，控制表格显示的列
      columns: [],
      columnss: [],
      userInfo: null,
      refKeys: ['ref0', 'ref1'],
      dateDisable:false,
    }
  },
  methods: {
    loadDataUpload(arr) {
      this.jdemoshow = true
      for (var i = 0; i < arr.length; i++) {
        this.columnss[i].dataSource = arr[i]
      }

      this.jdemoshow = false
    },
    getDatas() {
      this.getDatass().then(tables => {
        var arr = []
        console.log(tables)
        for(var i = 0 ; i < tables.length;i++) {
          arr.push(tables[i][0].validAllEvent())
        }
        for(var i = 0 ;i< arr.length;i++) {
          for(var j  =0;j< arr[i].length;j++) {
            if(arr[i][j].parent_id) {
              var id = arr[i][j].parent_id
            }else {
              var id = ''
            }
          }
        }
        let object = {
          id:"id",
          list:[[],[]]
        }
         var obj = {
              templateType: this.formData.template,
              reportFromType: this.formData.type,
              list:arr,
              id
            }
            // 【模拟保存】
            this.loading = true
            postAction(this.url.save, obj).then(res => {
              if (res.success) {
                this.$message.success(`保存成功！`)
              } else {
                this.$message.warn(`保存失败：` + res.message)
              }
            }).finally(() => {
              this.loading = false
            })
      })
    },
    getDatass() {
      var arr = ['ref0', 'ref1']
      let values = arr.map(key => getRefPromise(this, key))
      return Promise.all(values)
    }
    ,
    callback() {
    }
    ,
    // 获取模板类型
    getTemplateList() {
      var formData = {}
      getAction(this.url.templateUrl, formData).then(res => {
        if (res.success) {
          // 将查询的数据赋值给 dataSource
          this.templateList = res.result
          // 重置选择
        } else {
          this.$error({title: '主表查询失败', content: res.message})
        }
      }).finally(() => {
        // 这里是无论成功或失败都会执行的方法，在这里关闭loading
        this.loading = false
      })
      getAction(this.url.userInfo, formData).then(res => {
        if (res.success) {
          // 将查询的数据赋值给 dataSource
          this.userInfo = res.result
          // 重置选择
        } else {
          this.$error({title: '主表查询失败', content: res.message})
        }
      }).finally(() => {
        // 这里是无论成功或失败都会执行的方法，在这里关闭loading
        this.loading = false
      })
    }
    ,
    // 选择日期的回调
    onChange(date, dateString) {
      this.formData.createTime = dateString
      var e = this.formData.type
      window.localStorage.setItem('formData', JSON.stringify(this.formData))
      if (e == 'SynthesizeLogging') {
        this.loadData()
      } else if (e == 'DutyData') {
      } else if (e == 'WeekStatistics') {

      }
    }
    ,
    // 选择类型
    changeType(e) {
      this.jdemoshow = false
      var date = new Date();
      var seperator1 = "-";
      var year = date.getFullYear();
      var month = date.getMonth() + 1;
      var strDate = date.getDate();
      if (month >= 1 && month <= 9) {
        month = "0" + month;
      }
      if (strDate >= 0 && strDate <= 9) {
        strDate = "0" + strDate;
      }
      var currentdate = year + seperator1 + month + seperator1 + strDate;
      console.log(e)
      if (e == 'SynthesizeLogging') {
        this.istype = true
        this.formData.template = ''
        this.dateDisable = false
        this.importBtn = false
      } else if (e == 'DutyData') {
        this.formData.template = this.templateList[0].headId
        this.istype = false
        this.formData.createTime = currentdate
        this.dateDisable = true
      } else if (e == 'WeekStatistics') {
        this.formData.template = ''
        this.istype = true
        this.formData.createTime = currentdate
        this.dateDisable = true
        this.importBtn = true

      }
      window.localStorage.setItem('formData', JSON.stringify(this.formData))
      this.getStructure()
      this.loadData()
    }
    ,
    // 选择模板
    templateChange(e) {
      this.jdemoshow = true
      window.localStorage.setItem('formData', JSON.stringify(this.formData))
      this.getStructure()
      // 发送请求头部，请求数据的请求
    }
    ,
    changeUploadMethod(e) {
      console.log(e)
    }
    ,
    // 查询结构（table的头部）
    getStructure() {
      // 封装查询条件
      let formData = {
        reportFromType: this.formData.type,
        templateType: this.formData.template
      }
      // 调用查询数据接口
      this.loading = true
      getAction(this.url.structure, formData).then(res => {
        if (res.success) {
          if (this.formData.type != 'DutyData') {
            var result = res.result
            // 将查询的数据赋值给 dataSource
            var arr = []
            for (var i = 0; i < result.length; i++) {
              if (result[i].isShowForm == 1) {
                arr.push({
                  key: result[i].dbFieldName,
                  title: result[i].dbFieldTxt,
                  type: JVXETypes[result[i].fieldShowType],
                  defaultValue: result[i].fieldDefaultValue
                })
              }
            }
            this.jdemoshow = true
            this.columns = arr
            this.columnss = []
          } else {//执勤数据
            var result = res.result
            var columns = []
            for (var i = 0; i < result.length; i++) {
              var arr = []
              var obj = {
                name: result[i].sheetName,
                dataSource:[]
              }
              var results = result[i].fields
              for (var j = 0; j < results.length; j++) {
                if (results[j].isShowForm == 1) {
                  arr.push({
                    key: results[j].dbFieldName,
                    title: results[j].dbFieldTxt,
                    type: JVXETypes[results[j].fieldShowType],
                    defaultValue: results[j].fieldDefaultValue
                  })
                }
              }
              obj.columns = arr
              columns.push(obj)
            }
              this.columnss = columns
              this.jdemoshow = false
          }
        } else {
          this.$error({title: '主表查询失败', content: res.message})
        }
      }).finally(() => {
        // 这里是无论成功或失败都会执行的方法，在这里关闭loading
        this.loading = false
      })
    }
    ,
    // 查询数据
    loadData() {
      // 封装查询条件
      let formData = {
        createTime: this.formData.createTime
      }
      // 调用查询数据接口
      this.loading = true
      if (this.formData.type == 'SynthesizeLogging') {
        getAction(this.url.list, formData).then(res => {
          if (res.success) {
            // 将查询的数据赋值给 dataSource
            this.dataSource = res.result
            // 重置选择
            this.selectedRows = []
          } else {
            this.$error({title: '主表查询失败', content: res.message})
          }
        }).finally(() => {
          // 这里是无论成功或失败都会执行的方法，在这里关闭loading
          this.loading = false
        })
      } else {
        this.dataSource = []
      }
    }
  },
  created() {
    var date = new Date();
    var seperator1 = "-";
    var year = date.getFullYear();
    var month = date.getMonth() + 1;
    var strDate = date.getDate();
    if (month >= 1 && month <= 9) {
      month = "0" + month;
    }
    if (strDate >= 0 && strDate <= 9) {
      strDate = "0" + strDate;
    }
    var currentdate = year + seperator1 + month + seperator1 + strDate;
    this.formData.createTime = currentdate
  },
  mounted() {
    // 获取头部数据
    this.getStructure()
    // 获取数据（list）
    this.loadData()
    // 获取模板列表
    this.getTemplateList()
    window.localStorage.setItem('formData', JSON.stringify(this.formData))
  }
}
</script>
<style scoped>
@import '~@assets/less/common.less';
</style>