<template>
  <div class="center-flex-box">
    <div class="card-box" style="width: 20%">
      <div class="box-title">信息筛选</div>
      <div class="box-content">
        <a-form layout="inline">
          <a-row :gutter="24">
            <a-col :xl="24" :lg="24" :md="24" :sm="24">
              <a-form-item label="业务类型" style="width: 100%; display: flex">
                <j-dict-select-tag
                  style="width: 100%"
                  v-model="queryParam.sex"
                  placeholder="请选择业务类型"
                  dictCode="ReportType"
                />
              </a-form-item>
            </a-col>
          </a-row>
        </a-form>
        <a-button type="primary" block style="margin-top: 20px">
          确定
        </a-button>
        <div class="scroll-box">
          <div
            v-for="(item, index) in templateList"
            :key="index"
            :class="index == activeIndex ? 'sb-item active' : 'sb-item'"
            @click="toggletemplate(item, index)"
          >
            {{ item.modelName }}
          </div>
        </div>
      </div>
    </div>
    <div class="card-box" style="width: 60%">
      <div class="box-title">数据展示: 报文名称报文名称.doc</div>
      <div class="box-content" id="center" v-html="centerHTML"></div>
      <a-button
          type="primary"
          icon="download"
          @click="handleExportXls('报表模板表')"
      >导出</a-button>
    </div>
    <div class="card-box" style="width: 20%">
      <div class="box-title">数据库配置</div>
     
      <div class="box-content" v-if="isConfiguration">
        <div class="card-container">
          <a-tabs type="card" @change="callback">
            <a-tab-pane key="1" tab="已有数据">
              <div class="card-content">
                <div class="left">选择数据</div>
                <div class="right">
                  <a-tree :load-data="onLoadData" :tree-data="treeData" @select="select" />
                </div>
              </div>
              <a-form :label-col="{ span: 7 }" :wrapper-col="{ span: 10 }">
                <a-form-item
                  label="数据库地址"
                  style="width: 100%; display: flex"
                >
                  <j-dict-select-tag
                    style="width: 100%"
                    v-model="queryParam.sex"
                    placeholder="请选择业务类型"
                    dictCode="ReportType"
                  />
                </a-form-item>
                <a-form-item
                  label="选择数据库"
                  style="width: 100%; display: flex"
                >
                  <j-dict-select-tag
                    style="width: 100%"
                    v-model="queryParam.sex"
                    placeholder="请选择业务类型"
                    dictCode="ReportType"
                  />
                </a-form-item>
                <a-form-item label="SQL语句" style="width: 100%; display: flex">
                  <a-textarea
                    v-model="sql"
                    placeholder="请输入SQL语句"
                    :auto-size="{ minRows: 3, maxRows: 5 }"
                  />
                </a-form-item>
                <div class="ant-row" style="display: flex">
                  <div class="ant-col ant-col-7">
                    <a-button type="primary"> 测试 </a-button>
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
              <a-button type="primary" block> 确定 </a-button>
            </a-tab-pane>
            <a-tab-pane key="2" tab="新增数据">
              <a-form :label-col="{ span: 7 }" :wrapper-col="{ span: 10 }">
                <a-form-item
                  label="标签分类"
                  style="width: 100%; display: flex"
                >
                  <j-dict-select-tag
                    style="width: 100%"
                    v-model="queryParam.sex"
                    placeholder="请选择业务类型"
                    dictCode="ReportType"
                  />
                </a-form-item>
                <a-form-item
                  label="标签名称"
                  style="width: 100%; display: flex"
                >
                  <a-input placeholder="Basic usage" />
                </a-form-item>
                <a-form-item
                  label="数据库地址"
                  style="width: 100%; display: flex"
                >
                  <a-input placeholder="Basic usage" />
                </a-form-item>
                <a-form-item
                  label="数据库接口"
                  style="width: 100%; display: flex"
                >
                  <a-textarea
                    v-model="sql"
                    placeholder="请输入SQL语句"
                    :auto-size="{ minRows: 3, maxRows: 4 }"
                  />
                </a-form-item>
                <a-form-item label="SQL语句" style="width: 100%; display: flex">
                  <a-textarea
                    v-model="sql"
                    placeholder="请输入SQL语句"
                    :auto-size="{ minRows: 3, maxRows: 6 }"
                  />
                </a-form-item>
                <div class="ant-row" style="display: flex">
                  <div class="ant-col ant-col-7">
                    <a-button type="primary"> 测试 </a-button>
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
                <a-button style="margin-right: 10px">取消</a-button>
                <a-button type="primary"> 确定 </a-button>
              </div>
            </a-tab-pane>
          </a-tabs>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import JEditor from "../../components/jeecg/JEditor.vue";
import {downFile, getAction, postAction} from "@api/manage";

export default {
  name: "bbsc",
  components: {
    JEditor,
  },
  data() {
    return {
      centerHTML: "",
      url: {
        templateUrl: "/southDataModel/list",
		ParentUrl: "/southDataClassify/rootList",
        childrenUrl: "/southDataClassify/childList",        exportXlsUrl:'/southDataReport/createword'
      },
      isConfiguration: true,
      indexStyle: 1,
      queryParam: {},
      activeIndex: 0,
      sql:
        "const char *mysql-sqlstate(MYSQL*mysql)const char *mysql-sqlstate(MYSQL*mysql)const char *mysql-sqlstate(MYSQL*mysql)",
      templateList: [],
      treeData: [
        { title: "11111111", key: "0" },
        { title: "Expand to load", key: "1" },
        { title: "Tree Node", key: "2", isLeaf: true },
      ],
    };
  },
  created() {},
  watch: {
    treeData: {
      handler: function (val, oldVal) {
        this.treeData = val;
      },
      deep: true,
    },
  },
  mounted() {
    this.getTemplateList();
    this.getParentList();
    var as = document.getElementById("center").getElementsByTagName("a");
    for (var i = 0; i < as.length; i++) {
      as[i].onclick = function (e) {};
    }
    window.addEventListener('click',function(e) {
      var arrDom = document.querySelectorAll('.WordSection1 .replace')
      for(var i = 0;i< arrDom.length;i++) {
        arrDom[i].onclick=function(e) {
          console.log(e.target.id)
        }
      }

    })
  },
  methods: {
    select(id,e) {
      console.log(id)
      var istrue = e.node.dataRef.hasChild
      if(istrue != 1) {
        this.sql = e.node.dataRef.onlSql
      }
    },
    callback(e) {
      console.log(e);
    },
    // 获取左侧模板列表

    getTemplateList() {
      var params = {};
      getAction(this.url.templateUrl, params).then((res) => {
        console.log(res);
        if (res.success) {
          this.templateList = res.result.records;
          this.centerHTML = res.result.records[0].modelHtmlCententUrl;

        }
      });
    },
    handleExportXls(fileName){
      if(!fileName || typeof fileName != "string"){
        fileName = "导出文件"
      }
     let param = {
        content: document.querySelector('.WordSection1').innerText
     }
      // window.open(window._CONFIG['domianURL']+this.url.exportXlsUrl+'?content'+document.querySelector('.WordSection1').innerText)
      downFile(this.url.exportXlsUrl,param).then((data)=>{
        console.log(data)
        if (!data) {
          this.$message.warning("文件下载失败")
          return
        }
        if (typeof window.navigator.msSaveBlob !== 'undefined') {
          window.navigator.msSaveBlob(new Blob([data],{type: 'application/octet-stream'}), fileName+'.docx')
        }else{
          let url = window.URL.createObjectURL(new Blob([data],{type: 'application/octet-stream'}))
          let link = document.createElement('a')
          link.style.display = 'none'
          link.href = url
          link.setAttribute('download', fileName+'.docx')
          document.body.appendChild(link)
          link.click()
          document.body.removeChild(link); //下载完成移除元素
          window.URL.revokeObjectURL(url); //释放掉blob对象
        }
      })
    },
    onLoadData(treeNode) {
      return new Promise((resolve) => {
        if (treeNode.dataRef.children) {
          resolve();
          return;
        }
        var id = treeNode.dataRef.id;
        getAction(this.url.childrenUrl, { pdataId: id })
          .then((res) => {
            if (res.success) {
              var result = res.result.records;
              for (var i = 0; i < result.length; i++) {
                result[i].title = result[i].dataTypeName;
                result[i].key = result[i].id;
                result[i].isLeaf = true;
              }
              treeNode.dataRef.children = result;
              this.treeData = [...this.treeData];
              resolve();
            }
          })
          .finally(() => {});
      });
    },
    toggletemplate(obj, index) {
      if (index == this.activeIndex) {
      } else {
        this.activeIndex = index;
        this.centerHTML = obj.modelHtmlCententUrl;
      }
    },
    //右侧菜单
    getParentList() {
      getAction(this.url.ParentUrl, {}).then((res) => {
        console.log(res.result.records);
        // this.parentList = res.result.records[0].dataTypeName;
        if (res.success) {
          var result = res.result.records;
          for (var i = 0; i < result.length; i++) {
            result[i].title = result[i].dataTypeName;
            result[i].key = result[i].id;
          }
          this.treeData = result;
        }
      });
    },
  },
  beforeDestroy() {  // 实例销毁之前对点击事件进行解绑
    window.removeEventListener('click', function () {
      
    });
  }
};
</script>
<style lang="less" scoped>
.center-flex-box {
  display: flex;
  height: calc(100vh - 160px);
  justify-content: space-between;
  .card-box {
    height: 100%;
    box-sizing: border-box;
    background: #fff;
    overflow-y: auto;
    border: 1px solid #ddd;
  }
}
.box-title {
  background: rgba(0, 121, 254, 1);
  line-height: 2.6rem;
  color: #fff;
  font-size: 1.2rem;
  padding-left: 20px;
}
.box-content {
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
  padding: 0 10px;
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
  width: 100%;
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
/deep/.card-container
  .ant-tabs
  .ant-tabs-bar
  .ant-tabs-nav-container
  .ant-tabs-tab-active.ant-tabs-tab {
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