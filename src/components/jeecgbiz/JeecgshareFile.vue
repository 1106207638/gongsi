<template>
  <j-select-biz-component ref="jselectbizcomponent" :disabled="disabled" v-bind="configs" v-on="$listeners" @input="getValue" />
</template>

<script>
import JSelectBizComponent from './JSelectBizComponent'
import { deleteAction, getAction, downFile } from '@/api/manage'
import { filterObj } from '@/utils/util'

export default {
  name: 'JShareFile',
  components: { JSelectBizComponent },
  props: ['value', 'listUrl','disabled'],
  data() {
    return {
      settings: {
        name: '标签名称',
        displayKey: 'name',
        queryParamCode:'name',
        // returnKeys:['id','name'],
        listUrl: this.listUrl,
        queryParamText: '标签名称',
        columns: [
          { title: '标签名称', dataIndex: 'name', align: 'center', width: 100 }
        ],
      },
    }
  },
  computed: {
    configs() {
      return Object.assign({ value: this.value }, this.settings, this.$attrs)
    },
  },
  methods: {
    getValue(val) {
      this.$emit('change', val)
    },
  },
}
</script>

<style lang="scss" scoped></style>