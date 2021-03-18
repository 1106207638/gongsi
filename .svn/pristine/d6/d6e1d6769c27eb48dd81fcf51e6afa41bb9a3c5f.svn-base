<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="模板名称">
              <j-input
                placeholder="请输入模板名称"
                v-model="queryParam.modelName"
              ></j-input>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="创建日期">
              <j-date
                :show-time="true"
                date-format="YYYY-MM-DD HH:mm:ss"
                placeholder="请选择创建日期"
                v-model="queryParam.createTime"
              ></j-date>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <span
              style="float: left; overflow: hidden"
              class="table-page-search-submitButtons"
            >
              <a-button type="primary" @click="searchQuery" icon="search"
                >查询</a-button
              >
              <a-button
                type="primary"
                @click="searchReset"
                icon="reload"
                style="margin-left: 8px"
                >重置</a-button
              >
              <a @click="handleToggleSearch" style="margin-left: 8px">
                {{ toggleSearchStatus ? "收起" : "展开" }}
                <a-icon :type="toggleSearchStatus ? 'up' : 'down'" />
              </a>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-upload
        name="file"
        :showUploadList="false"
        :multiple="false"
        :headers="tokenHeader"
        :action="url.uploadUrl"
        @change="handlechange"
      >
        <a-button type="primary" icon="import">上传文件</a-button>
      </a-upload>
      <a-button
        type="primary"
        icon="download"
        @click="handleExportXls('报表模板表')"
        >导出</a-button
      >
      <!--      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" >-->
      <!--        <a-button type="primary" icon="import">导入</a-button>-->
      <!--      </a-upload>-->
      <!-- 高级查询区域 -->
      <j-super-query
        :fieldList="superFieldList"
        ref="superQueryModal"
        @handleSuperQuery="handleSuperQuery"
      ></j-super-query>
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel"
            ><a-icon type="delete" />删除</a-menu-item
          >
        </a-menu>
        <a-button style="margin-left: 8px">
          批量操作 <a-icon type="down"
        /></a-button>
      </a-dropdown>
    </div>

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择
        <a style="font-weight: 600">{{ selectedRowKeys.length }}</a
        >项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-table
        ref="table"
        size="middle"
        :scroll="{ x: true }"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{
          selectedRowKeys: selectedRowKeys,
          onChange: onSelectChange,
        }"
        class="j-table-force-nowrap"
        @change="handleTableChange"
      >
        <template slot="htmlSlot" slot-scope="text">
          <div v-html="text"></div>
        </template>
        <template slot="imgSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px; font-style: italic"
            >无图片</span
          >
          <img
            v-else
            :src="getImgView(text)"
            height="25px"
            alt=""
            style="max-width: 80px; font-size: 12px; font-style: italic"
          />
        </template>
        <template slot="fileSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px; font-style: italic"
            >无文件</span
          >
          <a-button
            v-else
            :ghost="true"
            type="primary"
            icon="download"
            size="small"
            @click="downloadFile(text)"
          >
            下载
          </a-button>
        </template>

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>

          <a-divider type="vertical" />
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down" /></a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a @click="handleDetail(record)">详情</a>
              </a-menu-item>
              <a-menu-item>
                <a-popconfirm
                  title="确定删除吗?"
                  @confirm="() => handleDelete(record.id)"
                >
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>
      </a-table>
    </div>

    <south-data-model-copy-modal
      ref="modalForm"
      @ok="modalFormOk"
    ></south-data-model-copy-modal>
  </a-card>
</template>

<script>
import "@/assets/less/TableExpand.less";
import { mixinDevice } from "@/utils/mixin";
import { JeecgListMixin } from "@/mixins/JeecgListMixin";
import SouthDataModelCopyModal from "./modules/SouthDataModelModal";
import JSuperQuery from "@/components/jeecg/JSuperQuery.vue";

export default {
  name: "SouthDataModelCopyList",
  mixins: [JeecgListMixin, mixinDevice],
  components: {
    SouthDataModelCopyModal,
    JSuperQuery,
  },
  data() {
    return {
      description: "报表模板表管理页面",
      // 表头
      columns: [
        {
          title: "#",
          dataIndex: "",
          key: "rowIndex",
          width: 60,
          align: "center",
          customRender: function (t, r, index) {
            return parseInt(index) + 1;
          },
        },
        {
          title: "模板名称",
          align: "center",
          dataIndex: "modelName",
        },
        {
          title: "文件名称",
          align: "center",
          dataIndex: "modelFileName",
        },
        {
          title: "文件大小",
          align: "center",
          dataIndex: "modelFileSize",
        },
        {
          title: "文件格式",
          align: "center",
          dataIndex: "modelFileType",
        },
        {
          title: "创建日期",
          align: "center",
          dataIndex: "createTime",
        },
        {
          title: "创建人",
          align: "center",
          dataIndex: "createBy",
        },
        {
          title: "操作",
          dataIndex: "action",
          align: "center",
          fixed: "right",
          width: 147,
          scopedSlots: { customRender: "action" },
        },
      ],
      url: {
        list: "southDataModel/list",
        delete: "southDataModel/delete",
        deleteBatch: "southDataModel/deleteBatch",
        exportXlsUrl: "southDataModel/exportXls",
        importExcelUrl: "southDataModel/importExcel",
        uploadUrl: window._CONFIG["domianURL"] + "southDataModel/putword",
      },
      dictOptions: {},
      superFieldList: [],
    };
  },
  created() {
    this.getSuperFieldList();
  },
  computed: {
    importExcelUrl: function () {
      return `${window._CONFIG["domianURL"]}/${this.url.importExcelUrl}`;
    },
  },
  methods: {
    handlechange(info) {
      if (info.file.status == "uploading") {
        this.loading = true;
      }
      if (info.file.status !== "uploading") {
        this.loading = false;
        console.log(info);

        var res = info.file.response;
        if (res.success) {
          this.$message(`${info.file.name}上传成功！`);
        } else {
          this.$message.error(res.message);
        }
      }
      if (info.file.status === "done") {
      } else if (info.file.status === "error") {
        this.$message.error(`${info.file.name}上传失败！`);
      }
    },
    initDictConfig() {},
    // if(putword.){

    // }
    getSuperFieldList() {
      let fieldList = [];
      fieldList.push({
        type: "string",
        value: "modelName",
        text: "模板名称",
        dictCode: "",
      });
      fieldList.push({
        type: "string",
        value: "modelFileName",
        text: "文件名称",
        dictCode: "",
      });
      fieldList.push({
        type: "string",
        value: "modelFileSize",
        text: "文件大小",
        dictCode: "",
      });
      fieldList.push({
        type: "string",
        value: "modelFileType",
        text: "文件格式",
        dictCode: "",
      });
      fieldList.push({
        type: "datetime",
        value: "createTime",
        text: "创建日期",
      });
      fieldList.push({
        type: "string",
        value: "createBy",
        text: "创建人",
        dictCode: "",
      });
      this.superFieldList = fieldList;
    },
  },
};
</script>
<style scoped>
@import "~@assets/less/common.less";
</style>