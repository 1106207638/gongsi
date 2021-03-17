<template>
  <div :class="boxClass">
    <!-- 工具按钮 -->
    <div class="j-vxe-tool-button div" :size="btnSize" v-if="isfirst">
      <slot v-if="showPrefix" name="toolbarPrefix" :size="btnSize"/>
      <a-button v-if="showAdd" icon="plus" @click="trigger('add')" :disabled="disabled" type="primary">新增</a-button>
      <a-button v-if="showSave" icon="save" @click="trigger('save')" :disabled="disabled">保存</a-button>
      <a-upload name="file" v-if="importBtn" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button icon="import">导入</a-button>
      </a-upload>
      <template v-if="selectedRowIds.length > 0">
        <a-popconfirm
          v-if="showRemove"
          :title="`确定要删除这 ${selectedRowIds.length} 项吗?`"
          @confirm="trigger('remove')"
        >
          <a-button icon="minus" :disabled="disabled">删除</a-button>
        </a-popconfirm>
        <template v-if="showClearSelection">
          <a-button icon="delete" @click="trigger('clearSelection')">清空选择</a-button>
        </template>
      </template>

      <slot v-if="showSuffix" name="toolbarSuffix" :size="btnSize"/>
      <a v-if="showCollapse" @click="toggleCollapse" style="margin-left: 4px">
        <span>{{ collapsed ? '展开' : '收起' }}</span>
        <a-icon :type="collapsed ? 'down' : 'up'"/>
      </a>
    </div>
  </div>
</template>

<script>
  import Vue from "vue";
  import {ACCESS_TOKEN} from "@/store/mutation-types";
  import {Modal} from "ant-design-vue";
  import store from "@/store";

  export default {
    name: 'JVxeToolbar',
    props: {
      toolbarConfig: Object,
      excludeCode: Array,
      importBtn:Boolean,
      size: String,
      disabled: Boolean,
      disabledRows: Object,
      selectedRowIds: Array,
      isfirst: {
        type: Boolean,
        required: false,
        default: true
      }
    },
    data() {
      return {
        tokenHeader: {'X-Access-Token': Vue.ls.get(ACCESS_TOKEN)},
        importExcelUrl:window._CONFIG['domianURL']+'/southTable',
        // 是否收起
        collapsed: true,
      }
    },
    computed: {
      boxClass() {
        return {
          'j-vxe-toolbar': true,
          'j-vxe-toolbar-collapsed': this.collapsed,
        }
      },

      btns() {
        let arr = this.toolbarConfig.btn || ['add', 'remove', 'clearSelection']
        let exclude = [...this.excludeCode]
        // TODO 需要将remove替换batch_delete
        // 系统默认的批量删除编码配置为 batch_delete 此处需要转化一下
        if(exclude.indexOf('batch_delete')>=0){
          exclude.add('remove')
        }
        // 按钮权限 需要去掉不被授权的按钮
        return arr.filter(item=>{
          return exclude.indexOf(item)<0
        })
      },
      slots() {
        return this.toolbarConfig.slot || ['prefix', 'suffix']
      },
      showPrefix() {
        return this.slots.includes('prefix')
      },
      showSuffix() {
        return this.slots.includes('suffix')
      },
      showAdd() {
        return this.btns.includes('add')
      },
      showSave() {
        return this.btns.includes('save')
      },
      showRemove() {
        return this.btns.includes('remove')
      },
      showClearSelection() {
        if (this.btns.includes('clearSelection')) {
          // 有禁用行时才显示清空选择按钮
          // 因为禁用行会阻止选择行，导致无法取消全选
          let length = Object.keys(this.disabledRows).length
          return length > 0
        }
        return false
      },
      showCollapse() {
        return this.btns.includes('collapse')
      },

      btnSize() {
        return this.size === 'tiny' ? 'small' : null
      },
    },
    created() {
      var formData = JSON.parse(window.localStorage.getItem('formData'))
      console.log(formData)

      if(formData.type == 'WeekStatistics') {
        console.log(this.importExcelUrl)
        this.importExcelUrl+='/importWeekStrengthExcel'
      }else if(formData.type == 'DutyData') {
        this.importExcelUrl+='/importSheetsExcel'+'?id='+formData.template
      }
    },
    methods: {
      handleImportExcel(info){
        if (info.file.status !== 'uploading') {
          console.log(info.file, info.fileList);
        }
        if (info.file.status === 'done') {
          if (info.file.response.success) {
            // this.$message.success(`${info.file.name} 文件上传成功`);
            if (info.file.response.code === 201) {
              let { message, result: { msg, fileUrl, fileName } } = info.file.response
              let href = window._CONFIG['domianURL'] + fileUrl
              this.$warning({
                title: message,
                content: (
                    <div>
                      <span>{msg}</span><br/>
                      <span>具体详情请 <a href={href} target="_blank" download={fileName}>点击下载</a> </span>
                    </div>
                )
              })
            } else {
              this.$message.success(info.file.response.message || `${info.file.name} 文件上传成功`)
              window.localStorage.setItem('uploadresult',JSON.stringify(info.file.response.result))
              this.trigger('import')

            }
          } else {
            this.$message.error(`${info.file.name} ${info.file.response.message}.`);
          }
        } else if (info.file.status === 'error') {
          if (info.file.response.status === 500) {
            let data = info.file.response
            const token = Vue.ls.get(ACCESS_TOKEN)
            if (token && data.message.includes("Token失效")) {
              Modal.error({
                title: '登录已过期',
                content: '很抱歉，登录已过期，请重新登录',
                okText: '重新登录',
                mask: false,
                onOk: () => {
                  store.dispatch('Logout').then(() => {
                    Vue.ls.remove(ACCESS_TOKEN)
                    window.location.reload();
                  })
                }
              })
            }
          } else {
            this.$message.error(`文件上传失败: ${info.file.msg} `);
          }
        }
      },
      /** 触发事件 */
      trigger(name) {
        this.$emit(name)
      },
      triggers(name,obj) {
        this.$emit(name,obj)
      },
      // 切换展开收起
      toggleCollapse() {
        this.collapsed = !this.collapsed
      },
    },
  }
</script>

<style lang="less">
  .j-vxe-toolbar-collapsed {
    [data-collapse] {
      display: none;
    }
  }

  .j-vxe-tool-button.div .ant-btn {
    margin-right: 8px;
  }
</style>