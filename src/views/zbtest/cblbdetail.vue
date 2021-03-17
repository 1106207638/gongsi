<template>
  <div class="center-flex-box">
    <div class="card-box" style="width:300px">
      <div class="box-title">报表信息</div>

      <div class="box-content">
        <div class="card">
          <div class="card-item">
            <div class="label">时间:</div>
            <div class="text"> 2021.9.11</div>
          </div>
          <div class="card-item">
            <div class="label">模板类型：</div>
            <div class="text">重要值勤事件</div>
          </div>
          <div class="card-item">
            <div class="label">报表名称：</div>
            <div class="text"> 重要值勤事件9.11</div>
          </div>
          <div class="card-item">
            <div class="label">席位：</div>
            <div class="text">席位1</div>
          </div>
        </div>
      </div>
    </div>
    
    <div class="card-box" style="flex:1;padding-left: 20px">
      <div class="box-title">数据展示</div>
      <div class="box-content">
        <div v-if="istable">
          <s-table
              ref="table"
              size="default"
              :columns="columns"
              :data="loadData"
          >
          </s-table>
          <div class="tbottom">
            <div class="bi">
              协调席：马化腾
            </div>
            <div class="bi">
              海上席：张一鸣
            </div>
            <div class="bi">
              防空席：李彦宏
            </div>
            <div class="bi">
              陆上席：马云
            </div>
            <div class="bi">
              防务席：黄峥
            </div>
            <div class="bi">
              防务席：黄兴
            </div>
          </div>
        </div>
        <div v-else>
          红头文件
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { JeecgListMixin } from '@/mixins/JeecgListMixin'
import {getServiceList} from "@api/manage";
import STable from '@/components/table/'


  export default {
    name: "cblbdetail",
    mixins:['JeecgListMixin'],
    components: {
      STable
    },
    data() {
      return {
        id:'',
        indexStyle:1,
        istable:true,
        columns: [
          {
            title: '时间',
            dataIndex: 'no'
          },
          {
            title: '事件',
            dataIndex: 'description'
          },
          {
            title: '具体与落实情况',
            dataIndex: 'callNo',
            needTotal: true,
            customRender: (text) => text + ' 次'
          },
          {
            title: '席位名称',
            dataIndex: 'status',
          },
          {
            title: '值班员',
            dataIndex: 'updatedA',

          },
          {
            title: '备注',
            dataIndex: 'updatedAt',

          }
        ],
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

    },
    mounted() {
      this.getIndex()
      },
    methods: {
      getIndex() {
        var id = this.$route.params.id
        this.id = id
        if(id == 1) {
          this.istable = true
        }else {
          this.istable = false
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
.card-item {
  display: flex;
  margin: 10px 0;
}
.card-item .label {
  width: 100px;
}
.tbottom {
  display: flex;
}
.bi {
  flex: 1;
  text-align: center;
}
</style>