<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :xl="6" :lg="6" :md="8" :sm="24">
            <a-form-item label="报表编码">
              <a-input placeholder="请输入报表编码" v-model="queryParam.code"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="6" :md="8" :sm="24">
            <a-form-item label="报表名字">
              <a-input placeholder="请输入报表名字" v-model="queryParam.name"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">录入</a-button>
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
<!--              <a-menu-item>配置地址</a-menu-item>-->
<!--              <a-menu-item>功能测试</a-menu-item>-->
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>
        <span slot="dbsync" slot-scope="text, record">
          <div v-if="text=='Y'" style="color: limegreen;">已同步</div>
          <div v-else style="color: red">未同步</div>
        </span>

      </a-table>
    </div>
    <selfcgreportModule ref="modalForm" @ok="modalFormOk"  />
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
// 引入编辑module
import selfcgreportModule from './modules/selfcgreportModule'
import {getAction} from "@api/manage";
//高级查询modal需要参数
export default {
  name: "selfcgreport",
  mixins: [JeecgListMixin, mixinDevice],
  components: {
    selfcgreportModule
  },
  computed: {

  },
  data() {
    return {
      // 表单的类型
      description: "在线报表配置管理页面",
      visible: !1,
      reportUrlText: "",
      columns: [{
        title: "报表名称",
        align: "center",
        dataIndex: "name"
      },
        {
          title: "编码",
          align: "center",
          dataIndex: "code"
        },
        {
          title: "查询SQL",
          align: "center",
          dataIndex: "cgrSql",
          scopedSlots: {
            customRender: "cgrSql"
          }
        },
        {
          title: "数据源",
          align: "center",
          dataIndex: "dbSource"
        },
        {
          title: "创建时间",
          align: "center",
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
      url: {
        list: "/online/cgreport/head/list",
        delete: "/online/cgreport/head/delete",
        deleteBatch: "/online/cgreport/head/deleteBatch",
        getParamsInfo: "/online/cgreport/api/getParamsInfo/",
        sqlType:'/sys/dataSource/options',
      },
      // 数据库的类型
      sqlType:[]
    }
  },
  methods: {
    popReportURL: function(e) {
      this.visible = !0,
          this.initReportUrlText(e)
    },
    // 获取数据库的类型
    getsqlType() {
      var obj = {}
      getAction(this.url.sqlType, obj).then((res) => {
        console.log(res)
        if (res.success) {
          this.sqlType = res.result
        } else {
          this.$message.warning(res.message);
        }
      }).finally(() => {
      })
    },
    handleCancel: function() {
      this.visible = !1,
          this.reportUrlText = ""
    },
    onCopyUrl: function() {
      var e = this,
          t = new c.a(".copy-this-text");
      t.on("success", (function() {
        t.destroy(),
            e.$message.success("复制成功"),
            e.handleCancel()
      })),
          t.on("error", (function() {
            e.$message.error("该浏览器不支持自动复制"),
                t.destroy()
          }))
    }
  },
  created() {
    this.getsqlType()
  },
}
</script>
<style scoped>
@import '~@assets/less/common.less';
</style>