<template>
  <div class="searchBar">
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
  </div>
</template>
<script>
export default {};
</script>
<style scoped>
</style>