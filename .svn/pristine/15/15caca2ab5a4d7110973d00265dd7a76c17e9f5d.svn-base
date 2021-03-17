<template>
  <a-card :bordered="false">

    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :xl="6" :lg="6" :md="8" :sm="24">
            <a-form-item label="表名">
              <a-input placeholder="请输入表名" v-model="queryParam.tableName"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="6" :md="8" :sm="24">
            <a-form-item label="表类型">
              <a-select
                  mode="multiple"
                  style="width: 100%"
                  placeholder="请选择"
                  @change="handleChange"
              >
                <a-select-option v-for="(item,index) in cateList" :key="index">
                  {{ item }}
                </a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="6" :md="8" :sm="24">
            <a-form-item label="表描述">
              <a-input placeholder="请输入表描述" v-model="queryParam.tableTxt"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
              <a-button type="primary" icon="filter" style="margin-left: 8px">高级查询</a-button>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd1" type="primary" icon="plus">新增</a-button>
<!--      <a-button @click="handleAdd1" type="primary" icon="highlight">自定义按钮</a-button>-->
<!--      <a-button @click="handleAdd1" type="primary" icon="strikethrough">JS增强</a-button>-->
<!--      <a-button @click="handleAdd1" type="primary" icon="tool">JAVA增强</a-button>-->
<!--      <a-button @click="handleAdd1" type="primary" icon="database">导入数据库表</a-button>-->
<!--      <a-button @click="codeGeneration" type="primary" icon="database">代码生成</a-button>-->
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel">
            <a-icon type="delete"/>
            删除
          </a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px"> 批量操作
          <a-icon type="down"/>
        </a-button>
      </a-dropdown>
    </div>
    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{
          selectedRowKeys.length
        }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-table
          ref="table"
          size="middle"
          bordered
          rowKey="id"
          :columns="columns"
          :dataSource="dataSource"
          :pagination="ipagination"
          :loading="loading"
          :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
          class="j-table-force-nowrap"
          @change="handleTableChange">

        <template slot="htmlSlot" slot-scope="text">
          <div v-html="text"></div>
        </template>
        <template slot="imgSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无图片</span>
          <img v-else :src="getImgView(text)" height="25px" alt=""
               style="max-width:80px;font-size: 12px;font-style: italic;"/>
        </template>
        <template slot="fileSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无文件</span>
          <a-button
              v-else
              :ghost="true"
              type="primary"
              icon="download"
              size="small"
              @click="uploadFile(text)">
            下载
          </a-button>
        </template>
        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>
          <a-divider type="vertical"/>
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down"/></a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
              <a-menu-item @click="database(record)" v-if="record.isDbSynch!='Y'">同步数据库</a-menu-item>
<!--              <a-menu-item>权限控制</a-menu-item>-->
<!--              <a-menu-item>角色授权</a-menu-item>-->
<!--              <a-menu-item>生成视图</a-menu-item>-->
<!--              <a-menu-item>移除</a-menu-item>-->
<!--              <a-menu-item>删除</a-menu-item>-->
<!--              <a-menu-item>功能测试</a-menu-item>-->
<!--              <a-menu-item>配置地址</a-menu-item>-->
<!--              <a-menu-item>视图管理</a-menu-item>-->
            </a-menu>
          </a-dropdown>
        </span>
        <span slot="dbsync" slot-scope="text, record">
          <div v-if="text=='Y'" style="color: limegreen;">已同步</div>
          <div v-else style="color: red">未同步</div>
        </span>

      </a-table>
    </div>
    <formModule ref="modalForm" @ok="modalFormOk" />
    <codeGeneration ref="code" @ok="modalFormOk"  />
    <dataBase ref="dataBase" @ok="modalFormOk" />
  </a-card>
</template>

<script>
import {
  initDictOptions,
  filterDictText,
  filterDictTextByCache
} from '@/components/dict/JDictSelectUtil'
import {JeecgListMixin} from '@/mixins/JeecgListMixin'
import {mixinDevice} from '@/utils/mixin'

import Vue from 'vue'
import {filterObj} from '@/utils/util';
import JDemo from '../zbtest/JDemo.vue';
import UploadDemo from '../zbtest/uploadDemo.vue';
// 引入编辑页面
import formModule from './modules/formmodule'
// 代码生成
import codeGeneration from './modules/codeGeneration'
// 同步数据库
import dataBase from './modules/dataBase'
import {getAction} from "@api/manage";
//高级查询modal需要参数
const superQueryFieldList = [
  {
    type: "string",
    value: "name",
    text: "用户名"
  }, {
    type: "int",
    value: "age",
    text: "年龄"
  }, {
    type: "date",
    value: "birthday",
    text: "生日"
  }
]
export default {
  name: "JeecgDemoList",
  mixins: [JeecgListMixin, mixinDevice],
  components: {
    JDemo,
    UploadDemo,
    formModule,
    codeGeneration,
    dataBase
  },
  computed: {
    importExcelUrl: function () {
      return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
    },
  },
  data() {
    return {
      modal:false,
      // 表单的类型
      cateList: ['单表', '主表', '附表'],
      description: "Online表单开发管理页面",
      columns: [{
        title: "#",
        dataIndex: "",
        key: "rowIndex",
        width: 60,
        align: "center",
        customRender: function(e, t, n) {
          return parseInt(n) + 1
        }
      },
        {
          title: "表类型",
          align: "center",
          sorter: !0,
          dataIndex: "tableType",
          customRender: function(t, n) {
            console.log(t)
            console.log(n)
            if(t == 2) {
              return '主表'
            }else if(t == 3 ) {
              return '附表'
            }
          }
        },
        {
          title: "表名",
          sorter: !0,
          align: "center",
          dataIndex: "tableName"
        },
        {
          title: "表描述",
          align: "center",
          dataIndex: "tableTxt"
        },
        {
          title: "版本",
          align: "center",
          dataIndex: "tableVersion"
        },
        {
          title: "同步状态",
          align: "center",
          sorter: !0,
          dataIndex: "isDbSynch",
          scopedSlots: {
            customRender: "dbsync"
          }
        },
        {
          title: "创建时间",
          align: "center",
          sorter: !0,
          dataIndex: "createTime"
        },
        {
          title: "操作",
          dataIndex: "action",
          align: "center",
          scopedSlots: {
            customRender: "action"
          }
        }],
      superQueryFlag: !1,
      superQueryParams: "",
      superQueryFieldList: [{
        type: "input",
        value: "tableName",
        text: "表名"
      },
        {
          type: "select",
          value: "tableType",
          text: "表类型",
          dictCode: "cgform_table_type"
        },
        {
          type: "select",
          value: "formCategory",
          text: "表单业务分类",
          dictCode: "ol_form_biz_type"
        },
        {
          type: "select",
          value: "isDbSynch",
          text: "同步状态",
          options: [{
            label: "已同步",
            value: "Y"
          },
            {
              label: "未同步",
              value: "N"
            }]
        },
        {
          type: "date",
          value: "createTime",
          text: "创建时间"
        },
        {
          type: "input",
          value: "tableTxt",
          text: "表描述"
        }],
      url: {
        list: "southTable/list",
        delete: "/online/cgform/head/delete",
        deleteBatch: "/online/cgform/head/deleteBatch",
        doDbSynch: "/online/cgform/api/doDbSynch/",
        removeRecord: "/online/cgform/head/removeRecord",
        copyOnline: "/online/cgform/head/copyOnline",
        form:"/online/cgform/head/tableInfo",
      },
      tableTypeDictOptions: [],
      sexDictOptions: [],
      syncModalVisible: !1,
      syncFormId: "",
      synMethod: "normal",
      syncLoading: !1,
      onlineUrlTitle: "",
      onlineUrlVisible: !1,
      onlineUrl: "",
      selectedRowKeys: [],
      selectedRows: [],
      confirmVisible: !1
    }
  },
  methods: {
    handleAdd1: function () {
      var record  = {
        formCategory:"temp",
        formTemplate:'1',
        isCheckbox:'Y',
        isDesForm:'N',
        isPage:'Y',
        isTree:'N',
        scroll:1,
        tableName: '',
        tableTxt:'',
        tableType:2,
        idType:"UUID",
        queryMode:'single',
      }
      this.$refs.modalForm.edit(record);
      this.$refs.modalForm.title = "编辑";
      this.$refs.modalForm.disableSubmit = false;
    },
    // 同步数据库
    database(records) {
      this.$refs.dataBase.edit(records)
      this.$refs.dataBase.title = '同步数据库';
      this.$refs.dataBase.disableSubmit = false;
    },
    // 代码生成
    codeGeneration() {
      if(this.selectedRowKeys.length!=1) {
        this.$message.warning('请选择一条数据');
      }else {
        var httpurl = this.url.form
        var obj = {
          code: this.selectedRowKeys[0]
        }
        getAction(httpurl, obj).then((res) => {
          console.log(res)
          if (res.success) {
            if(res.result.main.isDbSynch!='Y') {
              this.$message.warning('请先同步数据库，在进行代码生成！');
            }else {
              this.$refs.code.add(this.selectedRowKeys[0])
              this.$refs.code.title = "代码生成";
              this.$refs.code.disableSubmit = false;
            }
          } else {

          }
        }).finally(() => {
        })
      }

    },
    handleChange(value) {
      this.queryParam.tableType_MultiString = value.toString()
    },
    onBirthdayChange: function (value, dateString) {
      console.log(dateString[0], dateString[1]);
      this.queryParam.birthday_begin = dateString[0];
      this.queryParam.birthday_end = dateString[1];
    },
    changeUploadType(e) {
      console.log(e)
      if (e < 2) {
        console.log(e)
        this.isshowlist = true
      } else {
        this.isshowlist = false
      }
    },
    changeUploadMethod(e) {
      console.log(e)
    }
  },
  created() {

  },
}
</script>
<style scoped>
@import '~@assets/less/common.less';
</style>