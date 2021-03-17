<template>
  <j-vxe-table
      :toolbar="toolbar"
      :isonline="isonline"
      :toolbarConfig="toolbarConfig"
      keep-source
      row-number
      ref="xTable"
      async-remove
      :drag-sort="issort"
      :height="366"
      :row-selection="issort"
      :loading="loading"
      :columns="columns"
      :dataSource="datas"
      :disabledRowIds="[1]"
      @save="handleTableSave"
      @remove="handleTableRemove"
      @edit-closed="handleEditClosed"
      @pageChange="handlePageChange"
      @selectRowChange="handleSelectRowChange"
      @selfAdd="addd"
      @edit-actived="editActivedEvent"
  >
    <template v-slot:call="props">
      <a-time-picker :default-value="moment(props.value, 'HH:mm:ss')"    @openChange="handleOpenChange(props)" @change="change">

      </a-time-picker>
    </template>
  </j-vxe-table>
</template>

<script>
import { getAction, postAction, putAction } from '@api/manage'
import { JVXETypes } from '@/components/jeecg/JVxeTable'
import moment from 'moment';
import {JeecgListMixin} from "@/mixins/JeecgListMixin";
var that = this

// 即时保存示例
export default {
  name: 'JDemo',
  mixins:[JeecgListMixin],
  props:['columns','issort','datas','toolbar','isonline'],
  data() {
    return {
      open: false,
      indexprop:{},
      // 工具栏的按钮配置
      toolbarConfig: {
        // add 新增按钮；remove 删除按钮；clearSelection 清空选择按钮
        btn: ['add', 'save', 'remove', 'clearSelection']
      },
      actionButton:true,
      // 是否正在加载
      loading: false,
      // 分页器参数
      pagination: {
        // 当前页码
        current: 1,
        // 每页的条数
        pageSize: 10,
        // 数据总数（目前并不知道真实的总数，所以先填写0，在后台查出来后再赋值）
        total: 0,
      },
      // 选择的行
      selectedRows: [],
      // 数据源，控制表格的数据
      dataSource: [],
      // 列配置，控制表格显示的列
      nameDisabled:false,

      // 查询url地址
      url: {
        getData: '/mock/vxe/getData',
        // 模拟保存单行数据（即时保存）
        saveRow: '/mock/vxe/immediateSaveRow',
        // 模拟保存整个表格的数据
        saveAll: '/mock/vxe/immediateSaveAll',
        yanzheng:'/online/cgform/api/checkOnlyTable'
      },
    }
  },
  created() {
  },
  methods: {
    editActivedEvent ({ row, rowIndex }) {
      console.log(row)
      console.log(rowIndex)
      this.nameDisabled = rowIndex <= 6
    },
    addd() {
      this.$emit('add')
    },
    moment,
    validAllEvent () {
      this.$refs.xTable.validateTable().then(errMap => {
        if (errMap) {
          console.log('表单验证未通过：', {errMap})
          this.$message.error('验证未通过，请在控制台查看详细')
          this.$emit('ok1',false)
          this.$emit('ok2',false)
          this.$emit('ok3',false)
        } else {
          this.$message.success('验证通过')
          this.$emit('ok1',true)
          this.$emit('ok2',true)
          this.$emit('ok3',true)
        }
      })
    },
    change(time, timeString) {
      console.log(time, timeString);
      this.indexprop.value = timeString
      this.indexprop.params.call = timeString
      this.indexprop.row.call = timeString
    },
    handleOpenChange(props) {
      console.log(props)
      this.indexprop = props
    },
    handleClose(props) {
      console.log(1)
    },
    handleView(props) {
      // 参数介绍：
      // props.value          当前单元格的值
      // props.row            当前行的数据
      // props.rowId          当前行ID
      // props.rowIndex       当前行下标
      // props.column         当前列的配置
      // props.columnIndex    当前列下标
      // props.$table         vxe实例，可以调用vxe内置方法
      // props.target         JVXE实例，可以调用JVXE内置方法
      // props.caseId         JVXE实例唯一ID
      // props.scrolling      是否正在滚动
      // props.triggerChange  触发change事件，用于更改slot的值
      console.log(props, props)
    },

    handleDelete(props) {
      // 使用实例：删除当前操作的行
      props.target.removeRows(props.row)
    },
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
    // 【整体保存】点击保存按钮时触发的事件
    handleTableSave({$table, target}) {
      // 校验整个表格
      $table.validate().then((errMap) => {
        // 校验通过
        if (!errMap) {
          // 获取所有数据
          let tableData = target.getTableData()
          console.log('当前保存的数据是：', tableData)
          // 获取新增的数据
          let newData = target.getNewData()
          console.log('-- 新增的数据：', newData)
          // 获取删除的数据
          let deleteData = target.getDeleteData()
          console.log('-- 删除的数据：', deleteData)

          // 【模拟保存】
          this.loading = true
          postAction(this.url.saveAll, tableData).then(res => {
            if (res.success) {
              this.$message.success(`保存成功！`)
            } else {
              this.$message.warn(`保存失败：` + res.message)
            }
          }).finally(() => {
            this.loading = false
          })
        }
      })
    },

    // 触发单元格删除事件
    handleTableRemove(event) {

      // 把 event.deleteRows 传给后台进行删除（注意：这里不会传递前端逻辑新增的数据，因为不需要请求后台删除）
      console.log('待删除的数据: ', event.deleteRows)
      // 也可以只传ID，因为可以根据ID删除
      let deleteIds = event.deleteRows.map(row => row.id)
      console.log('待删除的数据ids: ', deleteIds)

      // 模拟请求后台删除
      this.loading = true
      window.setTimeout(() => {
        this.loading = false
        this.$message.success('删除成功')
        // 假设后台返回删除成功，必须要调用 confirmRemove() 方法，才会真正在表格里移除（会同时删除选中的逻辑新增的数据）
        event.confirmRemove()
      }, 1000)
    },

    // 单元格编辑完成之后触发的事件
    handleEditClosed(event) {
      let {$table, row, column} = event

      let field = column.property
      let cellValue = row[field]
      // 判断单元格值是否被修改
      if ($table.isUpdateByRow(row, field)) {
        // 校验当前行
        $table.validate(row).then((errMap) => {
          // 校验通过
          if (!errMap) {
            // 【模拟保存】
            console.log('即时保存数据：', row)
                this.$message.success(`"${column.title}"保存成功！`)
                this.$emit('editsave',row)
                // 局部更新单元格为已保存状态
                $table.reloadRow(row, null, field)
          }
        })
      }
    },

    // 当分页参数变化时触发的事件
    handlePageChange(event) {
      // 重新赋值
      this.pagination.current = event.current
      this.pagination.pageSize = event.pageSize
      // 查询数据
      this.loadData()
    },

    // 当选择的行变化时触发的事件
    handleSelectRowChange(event) {
      this.selectedRows = event.selectedRows
      this.$emit('handlerRows',this.selectedRows)
    },

  },
}
</script>

<style scoped>

</style>