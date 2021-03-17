<template>
  <div class="background">
    <!-- 检查任务执行详情 -->
    <div>
      <!-- 页面标题 -->
      <div>
        <div
          style="
            background-color: #0e71d7;
            height: 30px;
            width: 5px;
            display: inline-block;
          "
        ></div>
        <font style="font-size: 20px">查看作战任务详情</font>
      </div>

      <div class="layout" style="padding-top: 25px">
        <h1 style="font-weight: 800">检查任务执行详情</h1>

        <a-table
          style="margin-bottom: 24px"
          :columns="columns"
          :loading="false"
          :data-source="data"
          :bordered="true"
          :pagination="false"
        >
        </a-table>

        <!-- 检查结果回执 -->
        <h1 style="font-weight: 800">检查结果回执</h1>
        <div style="padding-left: 100px">
          <a-form>
            <a-form-item
              label="隶属上级单位"
              :labelCol="labelCol"
              :wrapperCol="wrapperCol"
            >
              <font class="form-item">{{ data[0].subordinateUnits }}</font>
            </a-form-item>

            <a-form-item
              label="发文时间"
              :labelCol="labelCol"
              :wrapperCol="wrapperCol"
            >
              <font class="form-item">{{ data[0].startTime }}</font>
            </a-form-item>

            <a-form-item
              label="接通时间"
              :labelCol="labelCol"
              :wrapperCol="wrapperCol"
              v-if="data[0].checkWay == 'View'"
            >
              <a-row>
                <a-col :span="4">
                     <div  class="form-item">{{ data[0].responseTime }}</div>
                </a-col>
              </a-row>
            </a-form-item>

            <a-form-item
              label="回执时间"
              :labelCol="labelCol"
              :wrapperCol="wrapperCol"
            >
              <font class="form-item">{{ data[0].endTime }}</font>
            </a-form-item>

            <a-form-item
              label="抽查过程记录"
              :labelCol="labelCol"
              :wrapperCol="wrapperCol"
            >
              <div class="form-item" style="width:600px;word-wrap:break-word;">{{ data[0].checkResult }}</div>
            </a-form-item>

            <a-form-item
              label="问题数量"
              :labelCol="labelCol"
              :wrapperCol="wrapperCol"
            >
              <font class="form-item">{{ data[0].checkResultStatus }}</font>
            </a-form-item>
          </a-form>
        </div>
      </div>
    </div>

    <!-- 数据对比详情 -->
    <div v-if="data[0].checkWay == 'OperationalData'">
      <div class="title">数据对比详情</div>
      <a-row>
        <a-col :span="12">
          <div style="padding-left: 30px">
            <h4 style="font-weight: 800; padding: 10px 0px">作战数据</h4>

            <!-- 作战数据 -->
            <div class="chuck">
              <a-table
                :columns="columns1"
                :data-source="data1"
                :pagination="false"
                bordered
              >
                <template slot="title">
                  <div style="text-align: center">{{ title }}</div>
                </template>

                <template slot="number" slot-scope="number">
                  <!-- <a-badge count="3">{{ number }}</a-badge> -->
                  {{ number }}
                </template>
              </a-table>
            </div>
          </div>
        </a-col>

        <a-col :span="12">
          <h4 style="font-weight: 800; padding: 10px 0px">比对异常数据</h4>
          <!-- 对比异常数据 -->
          <div class="chuck" style="border: 1px solid #a9a9a9">
            <div>
              <ul class="list">
                <li
                  :key="index"
                  v-for="(item, index) in errorData"
                  style="
                    padding-top: 20px;
                    padding-bottom: 30px;
                    font-size: 15px;
                    color: #000000;
                  "
                >
                  <span class="span">
                    <span class="number"> {{ index + 1 }}</span>
                  </span>

                  <span style="padding-left: 10px">{{ item.name }}</span>
                  <br />

                  <div style="padding-top: 20px">
                    <div style="font-size: 23px">联指数据</div>
                    <div
                      style="padding-top: 5px; font-size: 20px; color: #a9a9a9"
                    >
                      {{ item.name }} : {{ item.number1 }}
                    </div>
                  </div>

                  <div style="padding-top: 20px">
                    <div style="font-size: 23px">211K数据</div>
                    <div
                      style="padding-top: 5px; font-size: 20px; color: #a9a9a9"
                    >
                      {{ item.name }} : {{ item.number2 }}
                    </div>
                  </div>
                </li>
              </ul>
            </div>
          </div>
        </a-col>
      </a-row>
    </div>
  </div>
</template>

<script>
import STable from "@/components/table/";
import { getAction } from "@/api/manage";

export default {
  name: "cblbdetail",
  mixins: ["JeecgListMixin"],
  components: {
    STable,
  },
  data() {
    return {
      labelCol: {
        xs: { span: 24 },
        sm: { span: 5 },
      },
      wrapperCol: {
        xs: { span: 24 },
        sm: { span: 16 },
      },
      // 检查任务执行详情
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
      ],
      data: [],
      // 作战数据
      title: "XXXXXX任务作战数据表",
      columns1: [
        {
          title: "名称",
          dataIndex: "name",
          align: "center",
        },
        {
          title: "数量",
          dataIndex: "number",
          scopedSlots: { customRender: "number" },
          align: "center",
        },
        {
          title: "备注",
          dataIndex: "address",
          align: "center",
        },
      ],
      // 作战数据源
      data1: [
        {
          key: "1",
          name: "人员情况",
          number: "",
          address: "",
        },
        {
          key: "2",
          name: "总服役人数",
          number: "123",
          address: "London No. 1 Lake Park",
        },
        {
          key: "3",
          name: "炮兵",
          number: "12",
          address: "Sidney No. 1 Lake Park",
          error: "1",
        },
      ],
      // 对比异常数据
      errorData: [
        {
          name: "炮兵",
          number1: 123,
          number2: 121,
        },
        {
          name: "总服役人数",
          number1: 21,
          number2: 23,
        },
        {
          name: "炮兵",
          number1: 123,
          number2: 22,
        },
      ],
      id: this.$route.params.id,
      url: {
        queryById: "/southCheckTask/queryById",
      },
      result: [],
      time1: "",
      time2: "",
    };
  },
  created() {
    this.queryById();
  },
  methods: {
    queryById() {
      getAction(this.url.queryById + "?id=" + this.id, {}).then((res) => {
        if (res.result.records.length == 0) {
          this.$message.error("该任务详情并不存在,三秒后,跳往检查任务页!");
          var timer1 = window.setTimeout(function () {
            this.$router.push(`/dutyCheck`);
            window.clearTimeout(timer1);
          }, 3000);
          return;
        }
        this.data = res.result.records;
      });
    },
  },
  watch: {
    "$route.params.id": function (newVal, oldVal) {
      if (typeof newVal != "undefined") {
        getAction(this.url.queryById + "?id=" + newVal, {}).then((res) => {
          if (res.result.records.length == 0) {
            this.$message.error("该任务详情并不存在,请退回至值班检查确定该任务!");
            return;
          }
          this.data = res.result.records;
        });
      }
    },
  },
};
</script>
<style lang="less" scoped>
.background {
  background-color: #ffffff;
  width: 100%;
}
.layout {
  width: 90%;
  margin: 0 auto;
}

.form-item {
  margin-left: 30px;
}
.title {
  padding-left: 50px;
  font-size: 14px;
  color: #fff;
  text-align: left;
  background: rgb(14, 113, 215);
  height: 50px;
  line-height: 50px;
}
.chuck {
  width: 95%;
  height: 850px;
  background-color: #f6f6f6;
}
th.column-money,
td.column-money {
  text-align: right !important;
}

.list {
  list-style: none;
}
.span {
  border-radius: 50%;
  height: 25px;
  width: 25px;
  display: inline-block;
  background: #a52a2a;
  vertical-align: top;
}
.number {
  display: block;
  color: #ffffff;
  height: 20px;
  line-height: 20px;
  text-align: center;
}
</style>
