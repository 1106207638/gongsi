<template>
  <div class="center-flex-box">
    <div class="card-box" style="width:20%">
      <div class="box-title">信息筛选</div>
      <div class="box-content">
        <a-form layout="inline" >
          <a-row :gutter="24">
            <a-col :xl="24" :lg="24" :md="24" :sm="24">
              <a-form-item label="业务类型" style="width: 100%;display: flex">
                <j-dict-select-tag style="width: 100%"  v-model="queryParam.sex" placeholder="请选择业务类型" dictCode="ReportType"/>
              </a-form-item>
            </a-col>
          </a-row>
        </a-form>
        <a-button type="primary" block style="margin-top: 20px;">
          确定
        </a-button>
        <div class="scroll-box">
          <div v-for="(item,index) in sbList" :class="index == activeIndex?'sb-item active':'sb-item'" @click="toggleSb(index)">{{item}}</div>
        </div>
      </div>
    </div>
    <div class="card-box" style="width:60%">
      <div class="box-title">数据展示: 报文名称报文名称.doc</div>
      <div class="box-content" id="center">
        <p>123</p>
        <a data-point="1">x个</a>
        <p>123</p>
        <a  data-point="2">x个</a>
        <p>123</p>
        <a  data-point="3">x个</a>
        <p>123</p>
        <a  data-point="4">x个</a>
      </div>
    </div>
    <div class="card-box" style="width:20%">
      <div class="box-title">数据库配置</div>
      <div class="box-content">
        <div class="card-container">
          <a-tabs type="card"  @change="callback">
            <a-tab-pane key="1" tab="已有数据">
              <div class="card-content">
                <div class="left">选择数据</div>
                <div class="right">
                  <a-tree :load-data="onLoadData" :tree-data="treeData" />
                </div>
              </div>
              <a-form :label-col="{ span: 7 }" :wrapper-col="{ span: 10 }" >
                <a-form-item label="数据库地址" style="width: 100%;display: flex">
                  <j-dict-select-tag style="width: 100%"  v-model="queryParam.sex" placeholder="请选择业务类型" dictCode="ReportType"/>
                </a-form-item>
                <a-form-item label="选择数据库" style="width: 100%;display: flex">
                  <j-dict-select-tag style="width: 100%"  v-model="queryParam.sex" placeholder="请选择业务类型" dictCode="ReportType"/>
                </a-form-item>
                <a-form-item label="SQL语句" style="width: 100%;display: flex">
                  <a-textarea
                      v-model="sql"
                      placeholder="请输入SQL语句"
                      :auto-size="{ minRows: 3, maxRows: 5 }"
                  />
                </a-form-item>
                <div class="ant-row" style="display: flex">
                  <div class="ant-col ant-col-7">
                    <a-button type="primary">
                      测试
                    </a-button>
                  </div>
                  <div class="ant-col ant-col-10" style="flex: 1">
                    <a-textarea
                        v-model="sql"
                        disabled="true"
                        placeholder="请输入SQL语句"
                        :auto-size="{ minRows: 3, maxRows: 5 }"
                    />
                  </div>
                </div>
              </a-form>
              <a-button type="primary" block>
                确定
              </a-button>
            </a-tab-pane>
            <a-tab-pane key="2" tab="新增数据">

              <a-form :label-col="{ span: 7 }" :wrapper-col="{ span: 10 }" >
                <a-form-item label="标签分类" style="width: 100%;display: flex">
                  <j-dict-select-tag style="width: 100%"  v-model="queryParam.sex" placeholder="请选择业务类型" dictCode="ReportType"/>
                </a-form-item>
                <a-form-item label="标签名称" style="width: 100%;display: flex">
                  <a-input placeholder="Basic usage" />
                </a-form-item>
                <a-form-item label="数据库地址" style="width: 100%;display: flex">
                  <a-input placeholder="Basic usage" />
                </a-form-item>
                <a-form-item label="数据库接口" style="width: 100%;display: flex">
                  <a-textarea
                      v-model="sql"
                      placeholder="请输入SQL语句"
                      :auto-size="{ minRows: 3, maxRows: 4 }"
                  />
                </a-form-item>
                <a-form-item label="SQL语句" style="width: 100%;display: flex">
                  <a-textarea
                      v-model="sql"
                      placeholder="请输入SQL语句"
                      :auto-size="{ minRows: 3, maxRows: 6 }"
                  />
                </a-form-item>
                <div class="ant-row" style="display: flex">
                  <div class="ant-col ant-col-7">
                    <a-button type="primary">
                      测试
                    </a-button>
                  </div>
                  <div class="ant-col ant-col-10" style="flex: 1">
                    <a-textarea
                        v-model="sql"
                        disabled="true"
                        placeholder="请输入SQL语句"
                        :auto-size="{ minRows: 3, maxRows: 5 }"
                    />
                  </div>
                </div>
              </a-form>
              <div style="text-align: right">
                <a-button style="margin-right: 10px;">取消</a-button>
                <a-button type="primary">
                  确定
                </a-button>
              </div>
            </a-tab-pane>
          </a-tabs>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import JEditor from '../../components/jeecg/JEditor.vue'


  export default {
    name: "bbsc",
    components: {
      JEditor
    },
    data() {
      return {
        indexStyle:1,
      queryParam:{},
        activeIndex:0,
        sql:'const char *mysql-sqlstate(MYSQL*mysql)const char *mysql-sqlstate(MYSQL*mysql)const char *mysql-sqlstate(MYSQL*mysql)',
        sbList:['报表名称1','报表名称2','报表名称3','报表名称4','报表名称5','报表名称6'],
        treeData: [
          { title: 'Expand to load', key: '0' },
          { title: 'Expand to load', key: '1' },
          { title: 'Tree Node', key: '2', isLeaf: true },
        ],
      }
    },
    created() {

    },
    mounted() {
      var as = document.getElementById('center').getElementsByTagName('a')
      for(var i =  0  ; i < as.length;i++) {
        as[i].onclick=function(e) {
          console.log(e.target.dataset.point)
        }
      }
    },
    methods: {
      onLoadData(treeNode) {
        return new Promise(resolve => {
          if (treeNode.dataRef.children) {
            resolve();
            return;
          }
          setTimeout(() => {
            treeNode.dataRef.children = [
              { title: 'Child Node', key: `${treeNode.eventKey}-0` },
              { title: 'Child Node', key: `${treeNode.eventKey}-1` },
            ];
            this.treeData = [...this.treeData];
            resolve();
          }, 1000);
        });
      },
      callback(key) {
        console.log(key);
      },
      toggleSb(index) {
        if(index == this.activeIndex) {}else {
          this.activeIndex = index
        }
      }
    }
  }
</script>
<style lang="less" scoped>
.center-flex-box{
  display: flex;
  height: calc(100vh - 160px);
  justify-content:space-between;
  .card-box{
    height: 100%;
    box-sizing: border-box;
    background: #FFF;
    overflow-y: auto;
    border: 1px solid #ddd;
  }
}
.box-title{
  background: rgba(0, 121, 254, 1);
  line-height: 2.6rem;
  color: #fff;
  font-size: 1.2rem;
  padding-left: 20px;
}
.box-content{
  padding: 1.3rem;
}
/deep/.ant-form-item-control-wrapper {
  flex: 1;
}
.scroll-box {
  padding: 10px;
  margin-top: 10px;
  width: 100%;
  height: 500px;
  overflow: scroll;
  border: 1px solid #ccc;
}
.scroll-box .sb-item {
  padding:0  10px;
  cursor: pointer;
}
.sb-item.active {
  background: rgb(30, 144, 255);
  color: #fff;
}
.card-container {
  background: #f5f5f5;
  overflow: hidden;
  padding: 0;
}
.card-container > .ant-tabs-card > .ant-tabs-content {
  height: 120px;
  margin-top: -16px;
}
/deep/.ant-tabs-nav {
  width:100%
}
.card-container > .ant-tabs-card > .ant-tabs-content > .ant-tabs-tabpane {
  background: #fff;
  padding: 16px;
}
/deep/.ant-tabs-tab {
  width: 50%;
  text-align: center;
}

.card-container > .ant-tabs-card > .ant-tabs-bar {
  border-color: #fff;
}

.card-container > .ant-tabs-card > .ant-tabs-bar .ant-tabs-tab {
  border-color: transparent;
  background: transparent;
}

.card-container > .ant-tabs-card > .ant-tabs-bar .ant-tabs-tab-active {
  border-color: #fff;
}
/deep/.card-container .ant-tabs .ant-tabs-bar .ant-tabs-nav-container .ant-tabs-tab-active.ant-tabs-tab {
  background: rgba(0, 121, 254, 1) !important;
  color: #fff;
  border: none;
}
.card-content {
  width: 100%;
  display: flex;
}
.card-content .left {
  width: 65px;
  padding-top: 7px;
}
.card-content .right {
  flex: 1;
}
</style>