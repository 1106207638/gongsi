<template>
  <j-select-biz-component
    ref="jselectbizcomponent"
    :disabled="disabled"
    v-bind="configs"
    v-on="$listeners"
    @input="getValue"
  />
</template>

<script>
import JSelectBizComponent from "./JSelectBizComponent";
import { deleteAction, getAction, downFile } from "@/api/manage";
import { filterObj } from "@/utils/util";

export default {
  name: "JShareFile",
  components: { JSelectBizComponent },
  props: ["value", "listUrl", "disabled"],
  data() {
    return {
      settings: {
        name: "模板名称",
        displayKey: "tableTxt",
        queryParamCode: "tableTxt",
        returnKeys: ["id", "id"],
        listUrl: this.listUrl,
        queryParamText: "模板名称",
        columns: [
          {
            title: "模板名称",
            dataIndex: "tableTxt",
            align: "center",
            width: 100,
          },
        ],
      },
    };
  },
  computed: {
    configs() {
      return Object.assign({ value: this.value }, this.settings, this.$attrs);
    },
  },
  methods: {
    getValue(val) {
      this.$emit("change", val);
    },
  },
};
</script>

<style lang="scss" scoped></style>