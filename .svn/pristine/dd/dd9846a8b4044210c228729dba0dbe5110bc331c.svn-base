<template>
  <a-card :bordered="false">
    <!-- 查询 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="48">
          <a-col :xl="5" :lg="6" :md="8" :sm="24">
            <a-form-item label="检查方式">
              <j-dict-select-tag
                type="list"
                v-model="queryParam.checkWay"
                placeholder="请选择报告类型"
                dictCode="check_way"
              />
            </a-form-item>
          </a-col>
          <a-col :xl="5" :lg="6" :md="8" :sm="24">
            <a-form-item label="检查状态">
              <a-select
                style="width: 100%"
                placeholder="请选择任务状态"
                v-model="queryParam.checkStatus"
              >
                <a-select-option value="Check"> 已检查 </a-select-option>
                <a-select-option value="NotCheck"> 未检查 </a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :xl="5" :lg="6" :md="8" :sm="24">
            <a-form-item label="执行时间">
              <j-date
                v-model="queryParam.checkTime"
                style="width: 100%"
                placeholder="请输入更新日期"
                dateFormat="YYYY-MM-DD"
              />
            </a-form-item>
          </a-col>

          <a-col :xl="4" :lg="6" :md="(!advanced && 8) || 24" :sm="24">
            <a-button
              type="primary"
              style="height: 40px; width: 120px"
              @click="searchQuery"
              >搜索</a-button
            >
          </a-col>
        </a-row>
      </a-form>
    </div>

    <a-divider />

    <a-button
      type="primary"
      @click="add"
      style="height: 40px; width: 120px; margin-bottom: 20px"
    >
      添加
    </a-button>

    <!-- 表格Strat -->
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

      <span slot="action" slot-scope="text, record" style="text-align: center">
        <!-- 未检查 -->
        <div v-if="record.checkStatus == 'NotCheck'">
          <a @click="implement(record)" v-if="record.checkStatus == 'NotCheck'"
            >执行</a
          >

          <a-divider type="vertical" v-if="record.checkStatus == 'NotCheck'" />

          <a-popconfirm
            title="确定删除吗?"
            @confirm="() => handleDelete(record.id)"
          >
            <a>删除</a>
          </a-popconfirm>
        </div>

        <!-- 检查 -->
        <div v-if="record.checkStatus != 'NotCheck'">
          <a @click="handleEdit(record)">编辑</a>

          <a-divider type="vertical" v-if="record.checkStatus == 'Check'" />

          <a @click="look(record.id)" v-if="record.checkStatus == 'Check'"
            >查看</a
          >

          <a-divider type="vertical" />

          <a-popconfirm
            title="确定删除吗?"
            @confirm="() => handleDelete(record.id)"
          >
            <a>删除</a>
          </a-popconfirm>
        </div>
      </span>
    </a-table>
    <!-- 表格end -->
    <a-button style="height: 40px; width: 120px" @click="batchDel"
      >删除</a-button
    >

    <editionModal ref="detailForm" @ok="searchQuery" />
    <addModule ref="modalForm" @ok="searchQuery" />
  </a-card>
</template>

<script>
import "@/assets/less/TableExpand.less";
import ATextarea from "ant-design-vue/es/input/TextArea";
import AInput from "ant-design-vue/es/input/Input";
import { JeecgListMixin } from "@/mixins/JeecgListMixin";
import { mixinDevice } from "@/utils/mixin";
import moment from "moment";
import axios from "axios";
import JDictSelectTag from "@/components/dict/JDictSelectTag.vue";
import { getRoleList } from "@/api/manage";
import addModule from "./module/zbjcaddModal";
import editionModal from "./module/zbjceditionModal";
export default {
  name: "cblb",
  mixins: [JeecgListMixin, mixinDevice],
  components: {
    AInput,
    ATextarea,
    JDictSelectTag,
    addModule,
    editionModal,
  },
  data() {
    return {
      visibleCreateModal: false,
      visible: false,
      labelCol: {
        xs: { span: 24 },
        sm: { span: 5 },
      },
      wrapperCol: {
        xs: { span: 24 },
        sm: { span: 12 },
      },
      form: null,
      mdl: {},

      // 高级搜索 展开/关闭
      advanced: true,
      // 查询参数
      queryParam: {},
      // 表头
      columns: [
        {
          title: "任务编号",
          dataIndex: "checkTaskNo",
          align: "center",
        },
        {
          title: "执行时间",
          dataIndex: "checkTime",
          align: "center",
        },
        {
          title: "受检单位",
          dataIndex: "checkDept",
          align: "center",
        },
        {
          title: "检查方式",
          dataIndex: "checkWay_dictText",
          align: "center",
        },
        {
          title: "检查状态",
          dataIndex: "checkStatus_dictText",
          align: "center",
        },
        {
          title: "操作",
          dataIndex: "action",
          width: "140px",
          scopedSlots: { customRender: "action" },
        },
      ],
      url: {
        list: "/southCheckTask/list",
        delete: "/southCheckTask/delete",
        deleteBatch: "/southCheckTask/deleteBatch",
      },
      selectedRowKeys: [],
      selectedRows: [],
    };
  },
  created() {
    getRoleList({ t: new Date() });
  },
  methods: {
    // 执行
    implement(records) {
      this.$refs.modalForm.edit(records);
      // 根据检查方式 显示时间内容
      let time1 = "";
      let time2 = "";
      if (records.checkWay === "Message") {
        time1 = "发文时间";
        time2 = "回执时间";
      } else if (records.checkWay === "View") {
        time1 = "呼出时间";
        time2 = "到位时间";
      } else {
        time1 = "下发时间";
        time2 = "回执时间";
      }
      this.$refs.modalForm.time1 = time1;
      this.$refs.modalForm.time2 = time2;

      this.$refs.modalForm.title = "执行";
      this.$refs.modalForm.taskId = records.id; //当前任务项id
      this.$refs.modalForm.checkWay = records.checkWay; //检查方式
      this.$refs.modalForm.disableSubmit = false;
    },

    // 查看
    look(id) {
      this.$router.push(`/zbtest/zbjcLook/` + id);
    },
    //添加
    add(records) {
      console.log(records);
      this.$router.push(`/zbtest/zbjcAdd`);
    },
    // 编辑
    handleEdit(record) {
      this.$refs.detailForm.edit(record);
      this.$refs.detailForm.title = "编辑内容";
      this.$refs.detailForm.disableSubmit = false;
    },

    handleOk() {},

    //添加逻辑
    handleModalVisible(isVisible) {
      this.visibleCreateModal = isVisible;
    },
    handleCreateModalOk() {
      this.createForm.validateFields((err, fieldsValue) => {
        if (err) {
          return;
        }
        const description = this.createForm.getFieldValue("description");
        axios
          .post("/saveRule", {
            desc: description,
          })
          .then((res) => {
            this.createForm.resetFields();
            this.visibleCreateModal = false;
            this.loadRuleData();
          });
      });
    },
    handleCreateModalCancel() {
      this.visibleCreateModal = false;
    },

    onChange(row) {
      this.selectedRowKeys = row.selectedRowKeys;
      this.selectedRows = row.selectedRows;
      console.log(this.$refs.table);
    },
    toggleAdvanced() {
      this.advanced = !this.advanced;
    },

    resetSearchForm() {
      this.queryParam = {
        date: moment(new Date()),
      };
    },
  },
  watch: {
    $route: function (newVal, oldVal) {
      this.searchQuery();
    },
  },
};
</script>

<style scoped>
.from-item {
  margin-left: 10px;
}
</style>