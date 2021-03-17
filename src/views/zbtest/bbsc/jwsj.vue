<template>
  <div class="center-flex-box">
    <div class="card-box" style="width:20%">
      <div class="box-title">信息筛选</div>
      <div class="box-content"></div>
    </div>
    <div class="card-box" style="width:79.5%">
      <div class="box-title">信息筛选</div>
      <div class="box-content">
        <j-editor></j-editor>
      </div>
    </div>
  </div>
</template>

<script>
import JEditor from '../../../components/jeecg/JEditor.vue'


  export default {
    name: "jwsj",
    components: {
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