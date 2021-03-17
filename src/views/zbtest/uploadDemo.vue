<template>
    <div>
    
    <a-card style="width:40%; margin:0 auto;">
        <j-upload-drage></j-upload-drage>
        
    </a-card>

   <a-table :columns="columns" :data-source="data" style="margin-top:2rem" bordered rowKey="id" :pagination="false">
        <span slot="action" slot-scope="text, record">
          <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
            <a>删除</a>
          </a-popconfirm>
        </span>
    </a-table>
    <div style="margin-top:20px;text-align:center">
        <a-button type="primary">确定</a-button>
    </div>
    </div>
</template>

<script>
  import { getAction, postAction, putAction } from '@api/manage'
  import { JVXETypes } from '@/components/jeecg/JVxeTable'
import JUploadDrage from '../../components/jeecg/JUploadDrage.vue'

  // 即时保存示例
  export default {
  components: { JUploadDrage },
    name: 'uploadDemo',
    data() {
      return {
        columns: [
          {key: 'num',dataIndex: 'num', title: '序号', width: '80px'},
          {key: 'len',dataIndex: 'len', title: '报告编号', width: '80px'},
          {key: 'ton',dataIndex: 'ton', title: '报告名称', width: '420px'},
          {key: 'payer',dataIndex: 'payer', title: '上报人', width: '120px'},
          {key: 'trend',dataIndex: 'trend', title: '上报时间', width: '120px'},
          {key: 'action',title: '操作', width: '120px',scopedSlots: {customRender: 'action'},},
        ],
        data:[{
            num:1,
            len:'sta8000',
            ton:'名称',
            payer:'张一鸣',
            trend:'上报时间',
        }],
        pagination: {
          // 当前页码
          current: 1,
          // 每页的条数
          pageSize: 10,
          // 数据总数（目前并不知道真实的总数，所以先填写0，在后台查出来后再赋值）
          total: 0,
        },
        // 查询url地址
        url: {
          getData: '/mock/vxe/getData',
          // 模拟保存单行数据（即时保存）
          saveRow: '/mock/vxe/immediateSaveRow',
          // 模拟保存整个表格的数据
          saveAll: '/mock/vxe/immediateSaveAll',
        },
      }
    },
    created() {
      this.loadData()
    },
    methods: {

      // 加载数据
      loadData() {
        // 封装查询条件
        let formData = {
          pageNo: this.pagination.current,
          pageSize: this.pagination.pageSize
        }
        // 调用查询数据接口
        this.loading = true
        getAction(this.url.getData, formData).then(res => {
          if (res.success) {
            // 后台查询回来的 total，数据总数量
            this.pagination.total = res.result.total
            // 将查询的数据赋值给 dataSource
            this.dataSource = res.result.records
            // 重置选择
            this.selectedRows = []
          } else {
            this.$error({title: '主表查询失败', content: res.message})
          }
        }).finally(() => {
          // 这里是无论成功或失败都会执行的方法，在这里关闭loading
          this.loading = false
        })
      },

    },
  }
</script>

<style scoped>

</style>