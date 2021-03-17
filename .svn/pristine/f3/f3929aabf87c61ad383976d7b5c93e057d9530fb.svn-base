<template>
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
              <a-form-item label="业务类型" style="width: 100%;display: flex">
                <j-dict-select-tag style="width: 100%" @input="changeUploadType" v-model="queryParam.sex" placeholder="请选择采报类型" dictCode="ReportType"/>
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
          <div v-for="(item,index) in plainOptions" :key="index" class="check-box">
            <a-checkbox  @change="onChange" :checked="checkedList.indexOf(item)!=-1" :name="item">
              {{ item }}
            </a-checkbox>
            <span class="look"><span @click="xwLook(item)">查看</span></span>
          </div>
          <!--          <a-checkbox-group v-model="checkedList" :options="plainOptions" @change="onChange" />-->
        </div>
      </div>
    </div>
    <div class="card-box" style="width:79.5%">
      <div class="box-title">数据展示</div>
      <div class="box-content">
        <j-Editor/>
      </div>
    </div>
  </div>
</template>

<script>
const plainOptions = ['席位一', '席位二', '席位三','席位四','席位五'];
const defaultCheckedList = [];
import JEditor from '../../../components/jeecg/JEditor.vue'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'
import {getServiceList} from "@api/manage";
import moment from 'moment';

export default {
  name: "zhrz",
  mixins:['JeecgListMixin'],
  components: {
    JEditor
  },
  data() {
    return {
      checkedList: [],
      indeterminate: true,
      checkAll: false,
      plainOptions,
      id:'',
      dateFormat: 'YYYY/MM/DD',
      indexStyle:1,
      queryParam:{}
    }
  },
  created() {

  },
  mounted() {
  },
  methods: {
    // 查看席位
    xwLook(records) {
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
.card-item {
  display: flex;
  margin: 10px 0;
}
.card-item .label {
  width: 100px;
}
/deep/.ant-form-item-control-wrapper {
  flex: 1;
}
.tbottom {
  display: flex;
}
.bi {
  flex: 1;
  text-align: center;
}
/deep/.ant-checkbox-group-item {
  display: block;
}
.check-box {
  display: flex;
}
.check-box .look {
  text-align: right;
  flex: 1;
  color: #0079FE;
}
.check-box .look span {
  cursor: pointer;
}
</style>