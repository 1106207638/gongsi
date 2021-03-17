<template>
<div>
  <a-form-item label="报表名称" labelAlign="right" :labelCol="{span: 1, offset: 0}" :wrapperCol="{span: 4, offset: 0}" >
      <a-input
        v-model="reportname"
      />
    </a-form-item>
  <div class="center-flex-box">
    <div class="card-box" style="width:20%">
      <div class="box-title">信息筛选</div>
      <div class="box-content">
        <a-form layout="inline" >
          <a-row :gutter="24">
            <a-col :xl="24" :lg="24" :md="24" :sm="24">
              <a-form-item label="时间" :labelCol="{span: 4, offset: 0}" :wrapperCol="{span: 20, offset: 0}" >
                <a-range-picker
                    :ranges="{ '今天': [moment(), moment()] }"
                    :format="dateFormat"
                />
              </a-form-item>
            </a-col>
          </a-row>
          <a-row :gutter="24">
            <a-col :xl="24" :lg="24" :md="24" :sm="24">
              <a-form-item label="业务类型"  :labelCol="{span: 8, offset: 0}" :wrapperCol="{span: 14, offset: 0}" >
                <j-dict-select-tag @input="changeUploadType" v-model="queryParam.sex" placeholder="请选择采报类型" dictCode="ReportType" />
              </a-form-item>
            </a-col>
          </a-row>
        </a-form>
        <a-button type="primary" block style="margin-top: 20px;">
          确定
        </a-button>
        <a-divider></a-divider>
        <div>
          <div>
            <a-checkbox :indeterminate="indeterminate" :checked="checkAll" @change="onCheckAllChange">
              全部
            </a-checkbox>
          </div>
          <br />
          <div v-for="item in plainOptions" class="check-box">
            <a-checkbox  @change="onChange" :checked="checkedList.indexOf(item)!=-1" :name="item">
              {{ item }}
            </a-checkbox>
            <span class="look"><span>查看</span></span>
          </div>
          <!--          <a-checkbox-group v-model="checkedList" :options="plainOptions" @change="onChange" />-->
        </div>
      </div>
    </div>
    <!-- <div class="card-box" style="width:59%">
      <div class="box-title">信息筛选</div>
      <div class="box-content">
        
      </div>
    </div> -->
      <div class="card-box" style="width:32%">
      <div class="box-title">数据展示: 报文名称报文名称.doc</div>
      <div class="box-content">
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>啊啊大家阿莱克斯到家了<a point="q111">这是要标红的</a></p>
        <p><a point="q111">这是要标红的</a>撒大家是都看啦就是考虑到家了</p>
        <p>这是要标红的撒大家是都看啦就是考虑到家了</p>
        <p>这是要标红的撒大家是都看啦就是考虑到家了</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
        <p>asdjkalsjdlkajsdlkasjslkd</p>
      </div>
    </div>
    <div class="card-box" style="width:32%">
      <div class="box-title">数据展示：报文名称1111112.xls</div>
      <div class="box-content" id="list" style="padding:0"> 
        <a-table :columns="columns" :data-source="data" :pagination="false"  :scroll="{ x: 1800, y: 'calc(100vh - 390px)' }">
          
        </a-table>
      </div>
    </div>
    <div class="card-box" style="width:15%">
      <div class="box-title">对比问题清单</div>
      <div class="box-content">
        <ul class="question-list">
          <li @click="goTextLocal('q111',['20','7'])">
            <div class="count">1</div>
            <div class="contrast-title">报表名称报表名称.doc</div>
            <div class="contrast-info">B-53H轰炸机</div>
            <div class="contrast-title">报文名称111111A.xls</div>
            <div class="contrast-info">B-53H轰炸机</div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import JEditor from '../../../components/jeecg/JEditor.vue'
const plainOptions = ['席位一', '席位二', '席位三','席位四','席位五'];
const defaultCheckedList = [];
import moment from 'moment';
  export default {
    name: "bwdb",
    components: {
      JEditor
    },
    data() {
      return {
        reportname:"",
        columns : [
          { title: '单位',width: 120, dataIndex: 'name', key: 'name', fixed: 'left' },
          { title: '人员数量', width: 100, dataIndex: 'age', key: 'age', fixed: 'left' },
          { title: '人员组成', dataIndex: 'address', key: '1',},
          { title: '装备型号', dataIndex: 'address', key: '2',},
          { title: '装备数量', dataIndex: 'address', key: '3', },
          { title: '发生地点', dataIndex: 'address', key: '4',},
          { title: '发生时间', dataIndex: 'address', key: '5',},
          { title: '动作', dataIndex: 'address', key: '6',},
          { title: '发生批次', dataIndex: 'address', key: '7', },
          { title: '值班类型', dataIndex: 'address', key: '8' },
        ],
        data:[],
        checkedList: [],
        indeterminate: true,
        checkAll: false,
        plainOptions,
        id:'',
        dateFormat: 'YYYY/MM/DD',
        indexStyle:1,
        queryParam:{},
        // 加载数据方法 必须为 Promise 对象
        loadData: parameter => {
          return getServiceList(Object.assign(parameter, this.queryParam))
              .then(res => {
                return res.result
              })
        },
      }
    },
    created() {
      this.init()
    },
    methods: {
      goTextLocal(wordid,exclelocal){
        document.querySelector(`a[point=${wordid}]`).scrollIntoView({
          behavior: "smooth",
          block: "start",
          inline: "nearest",
        });
        setTimeout(()=>{
            let currPosition = exclelocal[0] * 52;
            document.querySelector("#list .ant-table-body").scrollTo({
              top:currPosition,
              behavior: "smooth"
            });
        },800)
        

      },
      init(){
        for (let i = 0; i < 100; i++) {
          this.data.push({
            key: i,
            name: `Edrward ${i}`,
            age: 32,
            address: `London Park no. ${i}`,
          });
        }
      },
      onChange(e) {
      if(e.target.checked) {
        this.checkedList.push(e.target.name)
      }else {
        for(var i = 0; i < this.checkedList.length;i++) {
          if(this.checkedList[i] == e.target.name) {
            this.checkedList.splice(i, 1)
          }
        }
      }
      this.indeterminate = !!this.checkedList.length && this.checkedList.length < plainOptions.length;
      this.checkAll = this.checkedList.length === plainOptions.length;
    },
    onCheckAllChange(e) {
      Object.assign(this, {
        checkedList: e.target.checked ? plainOptions : [],
        indeterminate: false,
        checkAll: e.target.checked,
      });
    },
    moment,
    }
  }
</script>
<style lang="less" scoped>
.center-flex-box{
  display: flex;
  height: calc(100vh - 280px);
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
  height: calc(100% - 3rem);
  overflow: auto;
}
.question-list{
  
  margin: 0;
  padding: 0;
  li{
    list-style: none;
    padding-left: 30px;
    position: relative;
    border-bottom: 1px solid #eee;
    padding-bottom:15px;
    .count{
      position: absolute;
      width: 20px;
      height: 20px;
      text-align: center;
      line-height: 20px;
      background: #ff4433;
      left: 0px;
      margin-top: 5px;
      border-radius: 25px;
      color: #fff;
    }
    .contrast-title{
      font-size: 1rem;
      line-height: 2rem;
    }
  }
}
</style>