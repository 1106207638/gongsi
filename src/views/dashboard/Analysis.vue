<template>
  <div>
    <index-chart v-if="indexStyle==1"></index-chart>
    <index-bdc v-if="indexStyle==2"></index-bdc>
    <index-task v-if="indexStyle==3"></index-task>
    <div style="width: 100%;text-align: right;margin-top: 20px">
      请选择首页样式：
      <a-radio-group v-model="indexStyle">
        <a-radio :value="1">统计图表</a-radio>
        <a-radio :value="2">统计图表2</a-radio>
        <a-radio :value="3">任务表格</a-radio>
      </a-radio-group>
    </div>
    
  </div>
</template>

<script>
  import IndexChart from './IndexChart'
  import IndexTask from "./IndexTask"
  import IndexBdc from './IndexBdc'
import JEditor from '../../components/jeecg/JEditor.vue'


  export default {
    name: "Analysis",
    components: {
      IndexChart,
      IndexTask,
      IndexBdc,
      JEditor
    },
    data() {
      return {
        indexStyle:1
      }
    },
    created() {

    },
    methods: {

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
</style>