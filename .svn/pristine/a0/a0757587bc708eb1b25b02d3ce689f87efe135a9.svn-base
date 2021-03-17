<template>
  <j-modal
      :title="title"
      :width="width"
      :visible="visible"
      :fullscreen="true"
      :confirmLoading="confirmLoading"
      switchFullscreen
      @ok="handleOk"
      @cancel="handleCancel"
      cancelText="关闭">
    <a-spin :spinning="confirmLoading">
      <a-form :form="form">
        <a-row :gutter="24">
          <a-col :xl="8" :lg="8" :md="8" :sm="8">
            <a-form-item label="表类型" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <j-dict-select-tag :moren="false" type="list" v-decorator="['tableType']" :trigger-change="true"
                 @change="handleChange"                dictCode="cgform_table_type" placeholder="请选择表类型" class="form-item"/>

            </a-form-item>
          </a-col>
          <a-col :xl="8" :lg="8" :md="8" :sm="8" v-if="isMain">
            <a-form-item label="模板编号" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['tableName',{
                 rules: [{ required: true, message: '请输入模板编号' },{
                  validator: compareToFirstPassword,
                 }]
              }]"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="8" :lg="8" :md="8" :sm="8" v-if="isMain">
            <a-form-item label="表描述" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['tableTxt',{
                 rules: [{ required: true, message: '请输入表描述' }]
              }]" placeholder="请输入表描述"></a-input>
            </a-form-item>
          </a-col>

          <a-col :xl="8" :lg="8" :md="8" :sm="8" v-if="!isMain">
            <a-form-item label="表名" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['tableName',{
                 rules: [{ required: true, message: '请输入表名' },{
                  validator: compareToFirstPassword,
                 }]
              }]"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="8" :lg="8" :md="8" :sm="8" v-if="!isMain">
            <a-form-item label="表描述" :labelCol="labelCol" :wrapperCol="wrapperCol">
              <a-input v-decorator="['tableTxt',{
                 rules: [{ required: true, message: '请输入表描述' }]
              }]" placeholder="请输入表描述"></a-input>
            </a-form-item>
          </a-col>

        </a-row>
<!--        <a-row :gutter="24">-->
<!--          <a-col :xl="8" :lg="8" :md="8" :sm="8">-->
<!--            <a-form-item label="表单分类" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
<!--              <j-dict-select-tag type="list" v-decorator="['formCategory']" :trigger-change="true"-->
<!--                                 dictCode="ol_form_biz_type" placeholder="请选择表类型" class="form-item"/>-->
<!--            </a-form-item>-->
<!--          </a-col>-->
<!--          <a-col :xl="8" :lg="8" :md="8" :sm="8">-->
<!--            <a-form-item label="主题策略" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
<!--              &lt;!&ndash;              <j-dict-select-tag type="list" v-decorator="['idType']" :trigger-change="true"&ndash;&gt;-->
<!--              &lt;!&ndash;                                 dictCode="theme_Template" placeholder="请选择检查结果" class="form-item"/>&ndash;&gt;-->
<!--              <a-select default-value="normal" >-->
<!--                <a-select-option value="normal">-->
<!--                  ID_WORKER(分布式自增)-->
<!--                </a-select-option>-->
<!--              </a-select>-->
<!--            </a-form-item>-->
<!--          </a-col>-->
<!--          <a-col :xl="8" :lg="8" :md="8" :sm="8">-->
<!--            <a-form-item label="使用表单设计" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
<!--              <j-dict-select-tag type="list" v-decorator="['isDesForm']" :trigger-change="true"-->
<!--                                 dictCode="y_n" placeholder="请选择检查结果" class="form-item"/>-->
<!--            </a-form-item>-->
<!--          </a-col>-->
<!--        </a-row>-->
<!--        <a-row :gutter="24">-->
<!--          <a-col :xl="8" :lg="8" :md="8" :sm="8">-->
<!--            <a-form-item label="主题模板" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
<!--              &lt;!&ndash;              <j-dict-select-tag type="list" v-decorator="['themeTemplate']" :trigger-change="true"&ndash;&gt;-->
<!--              &lt;!&ndash;                                 dictCode="theme__Template" placeholder="请选择检查结果" class="form-item"/>&ndash;&gt;-->
<!--              <a-select default-value="normal" disabled >-->
<!--                <a-select-option value="normal">-->
<!--                  默认主题-->
<!--                </a-select-option>-->
<!--              </a-select>-->
<!--            </a-form-item>-->
<!--          </a-col>-->
<!--          <a-col :xl="8" :lg="8" :md="8" :sm="8">-->
<!--            <a-form-item label="表单分格" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
<!--              <j-dict-select-tag type="list" v-decorator="['formTemplate']" :trigger-change="true"-->
<!--                                 dictCode="form_style" placeholder="请选择检查结果" class="form-item"/>-->
<!--            </a-form-item>-->
<!--          </a-col>-->
<!--          <a-col :xl="8" :lg="8" :md="8" :sm="8">-->
<!--            <a-form-item label="滚动条" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
<!--              <j-dict-select-tag type="list" v-decorator="['scroll']" :trigger-change="true"-->
<!--                                 dictCode="has_none" placeholder="请选择检查结果" class="form-item"/>-->
<!--            </a-form-item>-->
<!--          </a-col>-->
<!--        </a-row>-->
<!--        <a-row :gutter="24">-->
<!--          <a-col :xl="8" :lg="8" :md="8" :sm="8">-->
<!--            <a-form-item label="显示复选框" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
<!--              <j-dict-select-tag type="list" v-decorator="['isCheckbox']" :trigger-change="true"-->
<!--                                 dictCode="y_n" placeholder="请选择检查结果" class="form-item"/>-->
<!--            </a-form-item>-->
<!--          </a-col>-->
<!--          <a-col :xl="8" :lg="8" :md="8" :sm="8">-->
<!--            <a-form-item label="是否分页" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
<!--              <j-dict-select-tag type="list" v-decorator="['isPage']" :trigger-change="true"-->
<!--                                 dictCode="y_n" placeholder="请选择检查结果" class="form-item"/>-->
<!--            </a-form-item>-->
<!--          </a-col>-->
<!--          <a-col :xl="8" :lg="8" :md="8" :sm="8">-->
<!--            <a-form-item label="是否树" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
<!--              <j-dict-select-tag type="list" v-decorator="['isTree']" :trigger-change="true"-->
<!--                                 dictCode="y_n" placeholder="请选择检查结果" class="form-item"/>-->
<!--            </a-form-item>-->
<!--          </a-col>-->
<!--        </a-row>-->
      </a-form>
      <div>
        <a-tabs default-active-key="1" @change="callback" >
          <a-tab-pane key="1" tab="数据库属性">
            <div style="margin: 10px 0">
              <a-button  icon="plus" @click="selfadd" type="primary">新增</a-button>
              <template v-if="selectedRowIds.length > 0">
                <a-popconfirm
                    :title="`确定要删除这 ${selectedRowIds.length} 项吗?`"
                    @confirm="remove"
                >
                  <a-button icon="minus" >删除</a-button>
                </a-popconfirm>
                <!--              <template >-->
                <!--                <a-button icon="delete" @click="trigger('clearSelection')">清空选择</a-button>-->
                <!--              </template>-->
              </template>
            </div>
            <j-demo :columns="columns1" ref="demo1" @ok1="ok1" @editsave="saveEdit"  :datas="fields" @handlerRows="selectRows" :toolbar="false" :isonline="true" :issort="true"></j-demo>
          </a-tab-pane>
<!--         -->
          <a-tab-pane key="4" tab="外键" v-if="!isMain">
            <j-demo :columns="columns4" :datas="fields" :toolbar="false" :issort="false"></j-demo>
          </a-tab-pane>
<!--          <a-tab-pane key="5" tab="索引">-->
<!--            <div style="margin: 10px 0">-->
<!--              <a-button  icon="plus" @click="selfaddindex" type="primary">新增</a-button>-->
<!--              <template v-if="selectedRowIds.length > 0">-->
<!--                <a-popconfirm-->
<!--                    :title="`确定要删除这 ${selectedRowIds.length} 项吗?`"-->
<!--                    @confirm="remove"-->
<!--                >-->
<!--                  <a-button icon="minus" >删除</a-button>-->
<!--                </a-popconfirm>-->
<!--                &lt;!&ndash;              <template >&ndash;&gt;-->
<!--                &lt;!&ndash;                <a-button icon="delete" @click="trigger('clearSelection')">清空选择</a-button>&ndash;&gt;-->
<!--                &lt;!&ndash;              </template>&ndash;&gt;-->
<!--              </template>-->
<!--            </div>-->

<!--            <j-demo :columns="columns5" ref="demo5"  @editsave="saveEditindex"  :datas="indexs" :toolbar="false" :issort="false"></j-demo>-->

<!--          </a-tab-pane>-->
<!--          <a-tab-pane key="6" tab="查询配置">-->
<!--            <j-demo :columns="columns6" :datas="fields" :issort="false"></j-demo>-->

<!--          </a-tab-pane>-->
        </a-tabs>
      </div>
    </a-spin>
  </j-modal>

</template>

<script>

import {getAction, httpAction} from '@/api/manage'
import pick from 'lodash.pick'
import {validateDuplicateValue} from '@/utils/util'
import JDictSelectTag from "@/components/dict/JDictSelectTag"
import JDemo from './jform.vue';
import { JEditableTableMixin } from '@/mixins/JEditableTableMixin'
import {JVXETypes} from "@comp/jeecg/JVxeTable";


export default {
  name: "DictVueTestModal",
  // mixins: [JEditableTableMixin],
  components: {
    JDictSelectTag,
    JDemo
  },
  prop: {
    taskId: {
      type: String,
      required: true,
    }
  },
  data() {
    return {
      isMain:true,
      id:'',
      // 新增时子表默认添加几行空数据
      addDefaultRowNum: 1,
      // 选中的行数
      selectedRowIds:[],
      refKeys: ['DatabaseProperties','PageProperties','InspectionField','ForeignKey','indexes','QueryConfiguration'],
      tableKeys: ['DatabaseProperties','PageProperties','InspectionField','ForeignKey','indexes','QueryConfiguration'],
      activeKey: 'DatabaseProperties',
      // 数据库属性
      DatabasePropertiesTable:{
        loading: false,
        dataSource: [],
        columns: [{
          title: "字段名称",
          key: "dbFieldName",
          width: "160px",
          type: JVXETypes.input,
          defaultValue: "",
          placeholder: "请输入${title}",
          props: {
            disabled: false
          },
          validateRules: [{
            required: !0,
            trigger: "blur",
            message: "${title}不能为空"
          },
            {
              trigger: "change",
              pattern: /^[a-zA-Z]{1}(?!_)[a-zA-Z0-9_\\$]+$/,
              message: "命名规则:只能由字母、数字、下划线、$符号组成;必须以字母开头;不能以单个字母加下滑线开头"
            },
            {
              unique: true,
              trigger: "blur",
              message: '${title}不能重复'
            },
            {
              handler({cellValue, row, column}, callback, target) {

                callback(true) // true = 通过验证

              },
              message: '${title}默认提示'
            }],
        },
          {
            title: "字段备注",
            key: "dbFieldTxt",
            width: "140px",
            type: JVXETypes.input,
            defaultValue: "",
            placeholder: "请输入${title}",

          },
          {
            title: "字段长度",
            key: "dbLength",
            width: "100px",
            type: JVXETypes.inputNumber,
            defaultValue: 32,
            placeholder: "请输入${title}",

            props: {
              disabled: false
            }
          },
          {
            title: "小数点",
            key: "dbPointLength",
            width: "100px",
            type: JVXETypes.inputNumber,
            defaultValue: 0,
            placeholder: "请输入${title}",

            props: {
              disabled: false
            }
          },
          {
            title: "默认值",
            key: "dbDefaultVal",
            width: "130px",
            type: JVXETypes.input,
            defaultValue: "",
            props: {
              disabled: false
            }
          },
          {
            title: "字段类型",
            key: "dbType",
            width: "140px",
            type: JVXETypes.select,
            options: [{
              title: "String",
              value: "string"
            },
              {
                title: "Integer",
                value: "int"
              },
              {
                title: "Double",
                value: "double"
              },
              {
                title: "Date",
                value: "Date"
              },
              {
                title: "BigDecimal",
                value: "BigDecimal"
              },
              {
                title: "Text",
                value: "Text"
              },
              {
                title: "Blob",
                value: "Blob"
              }],
            defaultValue: "string",
            placeholder: "请选择${title}",
            props: {
              disabled: false
            },
          },
          {
            title: "主键",
            key: "dbIsKey",
            width: "80px",
            type: JVXETypes.checkbox,
            customValue: ["1", "0"],
            defaultChecked: !1,
            props: {
              disabled: false
            }
          },
          {
            title: "允许空值",
            key: "dbIsNull",
            width: "80px",
            type: JVXETypes.checkbox,
            customValue: ["1", "0"],
            defaultChecked: !0,
            props: {
              disabled: false
            }
          },
          {
            title: "排序",
            key: "orderNum",
            isOrder: !0,
            type: JVXETypes.hidden,
            defaultValue: 0
          }],
      },
      // 页面属性
      PagePropertiesTable:{
        loading: false,
        dataSource: [],
        columns: [{
          title: "字段名称",
          key: "dbFieldName",
          width: "100px",
          type: JVXETypes.input,
          defaultValue: "",
          placeholder: "",
          props: {
            disabled: !0
          }
        },
          {
            title: "字段备注",
            key: "dbFieldTxt",
            width: "100px",
            type: JVXETypes.input,
            defaultValue: "",
            placeholder: "",
            props: {
              disabled: !0
            }
          },
          {
            title: "表单显示",
            key: "isShowForm",
            width: "40px",
            type: JVXETypes.checkbox,
            customValue: ["1", "0"],
            defaultChecked: !0
          },
          {
            title: "列表显示",
            key: "isShowList",
            width: "40px",
            type: JVXETypes.checkbox,
            customValue: ["1", "0"],
            defaultChecked: !0
          },
          {
            title: "是否排序",
            key: "sortFlag",
            width: "40px",
            type: JVXETypes.checkbox,
            customValue: ["1", "0"],
            defaultChecked: !1
          },
          {
            title: "是否只读",
            key: "isReadOnly",
            width: "40px",
            type: JVXETypes.checkbox,
            customValue: ["1", "0"],
            defaultChecked: !1
          },
          {
            title: "控件类型",
            key: "fieldShowType",
            width: "170px",
            type: JVXETypes.select,
            defaultValue: "text",
            placeholder: "请选择${title}",
            validateRules: [{
              required: !0,
              message: "请选择${title}"
            }],
            options:  [{
              title: "文本框",
              value: "text"
            },
              {
                title: "日期(yyyy-MM-dd)",
                value: "date"
              },
              {
                title: "日期（yyyy-MM-dd HH:mm:ss）",
                value: "datetime"
              },
              {
                title: "时间（HH:mm:ss）",
                value: "time"
              },
              {
                title: "下拉框",
                value: "list"
              },
              {
                title: "下拉多选框",
                value: "list_multi"
              },
              {
                title: "下拉搜索框",
                value: "sel_search"
              },
              {
                title: "分类字典树",
                value: "cat_tree"
              },
              {
                title: "Popup弹框",
                value: "popup"
              },
              {
                title: "部门选择",
                value: "sel_depart"
              },
              {
                title: "用户选择",
                value: "sel_user"
              },
              {
                title: "省市区组件",
                value: "pca"
              },
              {
                title: "自定义树控件",
                value: "sel_tree"
              }]
          },
          {
            title: "控件长度",
            key: "fieldLength",
            width: "100px",
            type: JVXETypes.inputNumber,
            defaultValue: "120",
            placeholder: "请输入${title}",
            validateRules: [{
              required: !0,
              message: "${title}不能为空"
            }]
          },
          {
            title: "是否查询",
            key: "isQuery",
            width: "60px",
            type: JVXETypes.checkbox,
            customValue: ["1", "0"],
            defaultChecked: !1
          },
          {
            title: "查询类型",
            key: "queryMode",
            width: "110px",
            type: JVXETypes.select,
            options: [{
              title: "普通查询",
              value: "single"
            },
              {
                title: "范围查询",
                value: "group"
              }],
            defaultValue: "single",
            placeholder: "请选择${title}",
            validateRules: [{
              required: !0,
              message: "请选择${title}"
            }]
          },
          {
            title: "控件默认值",
            key: "fieldDefaultValue",
            width: "180px",
            type: JVXETypes.input,
            defaultValue: ""
          },
          {
            title: "扩展参数",
            key: "fieldExtendJson",
            width: "160px",
            type: JVXETypes.input,
            defaultValue: ""
          },
          {
            title: "自定义转换器",
            key: "converter",
            width: "160px",
            type: JVXETypes.input,
            defaultValue: ""
          }],
      },
      // 检验字段
      InspectionFieldTable:{
        loading: false,
        dataSource: [],
        columns: [{
          title: "字段名称",
          key: "dbFieldName",
          width: "130px",
          type: JVXETypes.input,
          defaultValue: "",
          placeholder: "",
          props: {
            disabled: !0
          }
        },
          {
            title: "字段备注",
            key: "dbFieldTxt",
            width: "130px",
            type: JVXETypes.input,
            defaultValue: "",
            placeholder: "",
            props: {
              disabled: !0
            }
          },
          {
            title: "字段Href",
            key: "fieldHref",
            width: "130px",
            type: JVXETypes.input,
            defaultValue: ""
          },
          {
            title: "验证规则",
            key: "fieldValidType",
            width: "170px",
            type: JVXETypes.select,
            allowInput: !0,
            options: [{
              title: "空",
              value: ""
            },
              {
                title: "唯一校验",
                value: "only"
              },
              {
                title: "6到16位数字",
                value: "n6-16"
              },
              {
                title: "6到16位任意字符",
                value: "*6-16"
              },
              {
                title: "网址",
                value: "url"
              },
              {
                title: "电子邮件",
                value: "e"
              },
              {
                title: "手机号码",
                value: "m"
              },
              {
                title: "邮政编码",
                value: "p"
              },
              {
                title: "字母",
                value: "s"
              },
              {
                title: "数字",
                value: "n"
              },
              {
                title: "整数",
                value: "z"
              },
              {
                title: "非空",
                value: "*"
              },
              {
                title: "6到18位字符串",
                value: "s6-18"
              },
              {
                title: "金额",
                value: "money"
              }],
            defaultValue: ""
          },
          {
            title: "校验必填",
            key: "fieldMustInput",
            width: "80px",
            type: JVXETypes.checkbox,
            customValue: ["1", "0"],
            defaultChecked: !1
          },
          {
            title: "字典Table",
            key: "dictTable",
            width: "130px",
            type: JVXETypes.input_pop,
            defaultValue: ""
          },
          {
            title: "字典Code",
            key: "dictField",
            width: "130px",
            type: JVXETypes.input,
            defaultValue: ""
          },
          {
            title: "字典Text",
            key: "dictText",
            width: "130px",
            type: JVXETypes.input,
            defaultValue: ""
          }],
      },
      // 外键
      ForeignKeyTable:{
        loading: false,
        dataSource: [],
        columns: [{
          title: "字段名称",
          key: "dbFieldName",
          width: "160px",
          type: JVXETypes.input,
          defaultValue: "",
          placeholder: "",
          props: {
            disabled: !0
          }
        },
          {
            title: "字段备注",
            key: "dbFieldTxt",
            width: "160px",
            type: JVXETypes.input,
            defaultValue: "",
            placeholder: "",
            props: {
              disabled: !0
            }
          },
          {
            title: "主表名",
            key: "mainTable",
            width: "120px",
            type: JVXETypes.input,
            defaultValue: ""
          },
          {
            title: "主表字段",
            key: "mainField",
            width: "120px",
            type: JVXETypes.input,
            defaultValue: ""
          }],
      },
      // 索引
      indexesTable:{
        loading: false,
        dataSource: [],
        columns: [{
          title: "索引名称",
          key: "indexName",
          width: "330px",
          type: JVXETypes.input,
          defaultValue: "",
          placeholder: "请输入${title}",
          validateRules: [{
            required: !0,
            message: "${title}不能为空"
          }]
        },
          {
            title: "索引栏位",
            key: "indexField",
            width: "330px",
            type: JVXETypes.list_multi,
            options:[],
            defaultValue: "",
            placeholder: "请选择${title}",
            validateRules: [{
              required: !0,
              message: "请选择${title}"
            }],
            props: {
              notFoundContent: "没有什么好选择的"
            }
          },
          {
            title: "索引类型",
            key: "indexType",
            width: "330px",
            type: JVXETypes.select,
            options: [{
              title: "normal",
              value: "normal"
            },
              {
                title: "unique",
                value: "unique"
              }],
            defaultValue: "normal",
            placeholder: "请选择${title}",
            validateRules: [{
              required: !0,
              message: "请选择${title}"
            }]
          }],
      },
      // 查询配置
      QueryConfigurationTable:{
        loading: false,
        dataSource: [],
        columns: [{
          title: "字段名称",
          key: "dbFieldName",
          width: "130px",
          type: JVXETypes.input,
          defaultValue: "",
          placeholder: "",
          props: {
            disabled: !0
          }
        },
          {
            title: "字段备注",
            key: "dbFieldTxt",
            width: "130px",
            type: JVXETypes.input,
            defaultValue: "",
            placeholder: "",
            props: {
              disabled: !0
            }
          },
          {
            title: "控件类型",
            key: "queryShowType",
            width: "170px",
            type: JVXETypes.select,
            defaultValue: "text",
            placeholder: "请选择${title}",
            options:  [{
              title: "文本框",
              value: "text"
            },
              {
                title: "日期(yyyy-MM-dd)",
                value: "date"
              },
              {
                title: "日期（yyyy-MM-dd HH:mm:ss）",
                value: "datetime"
              },
              {
                title: "时间（HH:mm:ss）",
                value: "time"
              },
              {
                title: "下拉框",
                value: "list"
              },
              {
                title: "下拉多选框",
                value: "list_multi"
              },
              {
                title: "下拉搜索框",
                value: "sel_search"
              },
              {
                title: "分类字典树",
                value: "cat_tree"
              },
              {
                title: "Popup弹框",
                value: "popup"
              },
              {
                title: "部门选择",
                value: "sel_depart"
              },
              {
                title: "用户选择",
                value: "sel_user"
              },
              {
                title: "省市区组件",
                value: "pca"
              },
              {
                title: "自定义树控件",
                value: "sel_tree"
              }]
          },
          {
            title: "字典Table",
            key: "queryDictTable",
            width: "130px",
            type: JVXETypes.input_pop,
            defaultValue: ""
          },
          {
            title: "字典Code",
            key: "queryDictField",
            width: "130px",
            type: JVXETypes.input,
            defaultValue: ""
          },
          {
            title: "字典Text",
            key: "queryDictText",
            width: "130px",
            type: JVXETypes.input,
            defaultValue: ""
          },
          {
            title: "默认值",
            key: "queryDefVal",
            width: "130px",
            type: JVXETypes.input,
            defaultValue: ""
          },
          {
            title: "是否启用",
            key: "queryConfigFlag",
            width: "80px",
            type: JVXETypes.checkbox,
            customValue: ["1", "0"],
            defaultChecked: !1
          }],
      },
      columns1: [{
        title: "字段名称",
        key: "dbFieldName",
        width: "160px",
        type: JVXETypes.input,
        defaultValue: "",
        placeholder: "请输入${title}",
        props: {
          disabled: false
        },
        validateRules: [{
          required: !0,
          trigger: "blur",
          message: "${title}不能为空"
        },
          {
            trigger: "change",
            pattern: /^[a-zA-Z]{1}(?!_)[a-zA-Z0-9_\\$]+$/,
            message: "命名规则:只能由字母、数字、下划线、$符号组成;必须以字母开头;不能以单个字母加下滑线开头"
          },
          {
            unique: true,
            trigger: "blur",
            message: '${title}不能重复'
          },
          {
            handler({cellValue, row, column}, callback, target) {

              callback(true) // true = 通过验证

            },
            message: '${title}默认提示'
          }],
      },
        {
          title: "字段备注",
          key: "dbFieldTxt",
          width: "140px",
          type: JVXETypes.input,
          defaultValue: "",
          placeholder: "请输入${title}",

        },
        {
          title: "字段长度",
          key: "dbLength",
          width: "100px",
          type: JVXETypes.inputNumber,
          defaultValue: 32,
          placeholder: "请输入${title}",

          props: {
            disabled: false
          }
        },
        {
          title: "小数点",
          key: "dbPointLength",
          width: "100px",
          type: JVXETypes.inputNumber,
          defaultValue: 0,
          placeholder: "请输入${title}",

          props: {
            disabled: false
          }
        },
        {
          title: "默认值",
          key: "dbDefaultVal",
          width: "130px",
          type: JVXETypes.input,
          defaultValue: "",
          props: {
            disabled: false
          }
        },
        {
          title: "字段类型",
          key: "dbType",
          width: "140px",
          type: JVXETypes.select,
          options: [{
            title: "String",
            value: "string"
          },
            {
              title: "Integer",
              value: "int"
            },
            {
              title: "Double",
              value: "double"
            },
            {
              title: "Date",
              value: "Date"
            },
            {
              title: "BigDecimal",
              value: "BigDecimal"
            },
            {
              title: "Text",
              value: "Text"
            },
            {
              title: "Blob",
              value: "Blob"
            }],
          defaultValue: "string",
          placeholder: "请选择${title}",
          props: {
            disabled: false
          },
        },
        {
          title: "控件类型",
          key: "fieldShowType",
          width: "170px",
          type: JVXETypes.select,
          defaultValue: "text",
          placeholder: "请选择${title}",
          validateRules: [{
            required: !0,
            message: "请选择${title}"
          }],
          options:  [{
            title: "文本框",
            value: "text"
          },
            {
              title: "密码",
              value: "password"
            },
            {
              title: "下拉框",
              value: "list"
            },
            {
              title: "单选框",
              value: "radio"
            },
            {
              title: "多选框",
              value: "checkbox"
            },
            {
              title: "开关",
              value: "switch"
            },
            {
              title: "日期(yyyy-MM-dd)",
              value: "date"
            },
            {
              title: "日期（yyyy-MM-dd HH:mm:ss）",
              value: "datetime"
            },
            {
              title: "时间（HH:mm:ss）",
              value: "time"
            },
            {
              title: "文件",
              value: "file"
            },
            {
              title: "图片",
              value: "image"
            },
            {
              title: "多行文本",
              value: "textarea"
            },
            {
              title: "下拉多选框",
              value: "list_multi"
            },
            {
              title: "下拉搜索框",
              value: "sel_search"
            },
            {
              title: "Popup弹框",
              value: "popup"
            },
            {
              title: "分类字典树",
              value: "cat_tree"
            },
            {
              title: "部门选择",
              value: "sel_depart"
            },
            {
              title: "用户选择",
              value: "sel_user"
            },
            {
              title: "富文本",
              value: "umeditor"
            },
            {
              title: "MarkDown",
              value: "markdown"
            },
            {
              title: "省市区组件",
              value: "pca"
            },
            {
              title: "联动组件",
              value: "link_down"
            },
            {
              title: "自定义树控件",
              value: "sel_tree"
            }],
        },
        {
          title: "主键",
          key: "dbIsKey",
          width: "80px",
          type: JVXETypes.checkbox,
          customValue: ["1", "0"],
          defaultChecked: !1,
          props: {
            disabled: false
          }
        },
        {
          title: "允许空值",
          key: "dbIsNull",
          width: "80px",
          type: JVXETypes.checkbox,
          customValue: ["1", "0"],
          defaultChecked: !0,
          props: {
            disabled: false
          }
        },
        {
          title: "排序",
          key: "orderNum",
          isOrder: !0,
          type: JVXETypes.hidden,
          defaultValue: 0
        },{
          title: "表单显示",
          key: "isShowForm",
          width: "80px",
          type: JVXETypes.checkbox,
          customValue: ["1", "0"],
          defaultChecked: !0
        },   {
          title: "控件默认值",
          key: "fieldDefaultValue",
          width: "180px",
          type: JVXETypes.input,
          defaultValue: ""
        },],
      columns2: [{
        title: "字段名称",
        key: "dbFieldName",
        width: "100px",
        type: JVXETypes.input,
        defaultValue: "",
        placeholder: "",
        props: {
          disabled: !0
        }
      },
        {
          title: "字段备注",
          key: "dbFieldTxt",
          width: "100px",
          type: JVXETypes.input,
          defaultValue: "",
          placeholder: "",
          props: {
            disabled: !0
          }
        },
        // {
        //   title: "表单显示",
        //   key: "isShowForm",
        //   width: "40px",
        //   type: JVXETypes.checkbox,
        //   customValue: ["1", "0"],
        //   defaultChecked: !0
        // },
        {
          title: "列表显示",
          key: "isShowList",
          width: "40px",
          type: JVXETypes.checkbox,
          customValue: ["1", "0"],
          defaultChecked: !0
        },
        {
          title: "是否排序",
          key: "sortFlag",
          width: "40px",
          type: JVXETypes.checkbox,
          customValue: ["1", "0"],
          defaultChecked: !1
        },
        {
          title: "是否只读",
          key: "isReadOnly",
          width: "40px",
          type: JVXETypes.checkbox,
          customValue: ["1", "0"],
          defaultChecked: !1
        },
        // {
        //   title: "控件类型",
        //   key: "fieldShowType",
        //   width: "170px",
        //   type: JVXETypes.select,
        //   defaultValue: "text",
        //   placeholder: "请选择${title}",
        //   validateRules: [{
        //     required: !0,
        //     message: "请选择${title}"
        //   }],
        //   options:  [{
        //     title: "文本框",
        //     value: "text"
        //   },
        //     {
        //       title: "日期(yyyy-MM-dd)",
        //       value: "date"
        //     },
        //     {
        //       title: "日期（yyyy-MM-dd HH:mm:ss）",
        //       value: "datetime"
        //     },
        //     {
        //       title: "时间（HH:mm:ss）",
        //       value: "time"
        //     },
        //     {
        //       title: "下拉框",
        //       value: "list"
        //     },
        //     {
        //       title: "下拉多选框",
        //       value: "list_multi"
        //     },
        //     {
        //       title: "下拉搜索框",
        //       value: "sel_search"
        //     },
        //     {
        //       title: "分类字典树",
        //       value: "cat_tree"
        //     },
        //     {
        //       title: "Popup弹框",
        //       value: "popup"
        //     },
        //     {
        //       title: "部门选择",
        //       value: "sel_depart"
        //     },
        //     {
        //       title: "用户选择",
        //       value: "sel_user"
        //     },
        //     {
        //       title: "省市区组件",
        //       value: "pca"
        //     },
        //     {
        //       title: "自定义树控件",
        //       value: "sel_tree"
        //     }]
        // },
        {
          title: "控件长度",
          key: "fieldLength",
          width: "100px",
          type: JVXETypes.inputNumber,
          defaultValue: "120",
          placeholder: "请输入${title}",
          validateRules: [{
            required: !0,
            message: "${title}不能为空"
          }]
        },
        {
          title: "是否查询",
          key: "isQuery",
          width: "60px",
          type: JVXETypes.checkbox,
          customValue: ["1", "0"],
          defaultChecked: !1
        },
        {
          title: "查询类型",
          key: "queryMode",
          width: "110px",
          type: JVXETypes.select,
          options: [{
            title: "普通查询",
            value: "single"
          },
            {
              title: "范围查询",
              value: "group"
            }],
          defaultValue: "single",
          placeholder: "请选择${title}",
          validateRules: [{
            required: !0,
            message: "请选择${title}"
          }]
        },
        // {
        //   title: "控件默认值",
        //   key: "fieldDefaultValue",
        //   width: "180px",
        //   type: JVXETypes.input,
        //   defaultValue: ""
        // },
        {
          title: "扩展参数",
          key: "fieldExtendJson",
          width: "160px",
          type: JVXETypes.input,
          defaultValue: ""
        },
        {
          title: "自定义转换器",
          key: "converter",
          width: "160px",
          type: JVXETypes.input,
          defaultValue: ""
        }],
      columns3: [{
        title: "字段名称",
        key: "dbFieldName",
        width: "130px",
        type: JVXETypes.input,
        defaultValue: "",
        placeholder: "",
        props: {
          disabled: !0
        }
      },
        {
          title: "字段备注",
          key: "dbFieldTxt",
          width: "130px",
          type: JVXETypes.input,
          defaultValue: "",
          placeholder: "",
          props: {
            disabled: !0
          }
        },
        {
          title: "字段Href",
          key: "fieldHref",
          width: "130px",
          type: JVXETypes.input,
          defaultValue: ""
        },
        {
          title: "验证规则",
          key: "fieldValidType",
          width: "170px",
          type: JVXETypes.select,
          allowInput: !0,
          options: [{
            title: "空",
            value: ""
          },
            {
              title: "唯一校验",
              value: "only"
            },
            {
              title: "6到16位数字",
              value: "n6-16"
            },
            {
              title: "6到16位任意字符",
              value: "*6-16"
            },
            {
              title: "网址",
              value: "url"
            },
            {
              title: "电子邮件",
              value: "e"
            },
            {
              title: "手机号码",
              value: "m"
            },
            {
              title: "邮政编码",
              value: "p"
            },
            {
              title: "字母",
              value: "s"
            },
            {
              title: "数字",
              value: "n"
            },
            {
              title: "整数",
              value: "z"
            },
            {
              title: "非空",
              value: "*"
            },
            {
              title: "6到18位字符串",
              value: "s6-18"
            },
            {
              title: "金额",
              value: "money"
            }],
          defaultValue: ""
        },
        {
          title: "校验必填",
          key: "fieldMustInput",
          width: "80px",
          type: JVXETypes.checkbox,
          customValue: ["1", "0"],
          defaultChecked: !1
        },
        {
          title: "字典Table",
          key: "dictTable",
          width: "130px",
          type: JVXETypes.input_pop,
          defaultValue: ""
        },
        {
          title: "字典Code",
          key: "dictField",
          width: "130px",
          type: JVXETypes.input,
          defaultValue: ""
        },
        {
          title: "字典Text",
          key: "dictText",
          width: "130px",
          type: JVXETypes.input,
          defaultValue: ""
        }],
      columns4: [{
        title: "字段名称",
        key: "dbFieldName",
        width: "160px",
        type: JVXETypes.input,
        defaultValue: "",
        placeholder: "",
        props: {
          disabled: !0
        }
      },
        {
          title: "字段备注",
          key: "dbFieldTxt",
          width: "160px",
          type: JVXETypes.input,
          defaultValue: "",
          placeholder: "",
          props: {
            disabled: !0
          }
        },
        {
          title: "主表名",
          key: "mainTable",
          width: "120px",
          type: JVXETypes.input,
          defaultValue: ""
        },
        {
          title: "主表字段",
          key: "mainField",
          width: "120px",
          type: JVXETypes.input,
          defaultValue: ""
        }],
      columns5: [{
        title: "索引名称",
        key: "indexName",
        width: "330px",
        type: JVXETypes.input,
        defaultValue: "",
        placeholder: "请输入${title}",
        validateRules: [{
          required: !0,
          message: "${title}不能为空"
        }]
      },
        {
          title: "索引栏位",
          key: "indexField",
          width: "330px",
          type: JVXETypes.list_multi,
          options:[],
          defaultValue: "",
          placeholder: "请选择${title}",
          validateRules: [{
            required: !0,
            message: "请选择${title}"
          }],
          props: {
            notFoundContent: "没有什么好选择的"
          }
        },
        {
          title: "索引类型",
          key: "indexType",
          width: "330px",
          type: JVXETypes.select,
          options: [{
            title: "normal",
            value: "normal"
          },
            {
              title: "unique",
              value: "unique"
            }],
          defaultValue: "normal",
          placeholder: "请选择${title}",
          validateRules: [{
            required: !0,
            message: "请选择${title}"
          }]
        }],
      columns6: [{
        title: "字段名称",
        key: "dbFieldName",
        width: "130px",
        type: JVXETypes.input,
        defaultValue: "",
        placeholder: "",
        props: {
          disabled: !0
        }
      },
        {
          title: "字段备注",
          key: "dbFieldTxt",
          width: "130px",
          type: JVXETypes.input,
          defaultValue: "",
          placeholder: "",
          props: {
            disabled: !0
          }
        },
        {
          title: "控件类型",
          key: "queryShowType",
          width: "170px",
          type: JVXETypes.select,
          defaultValue: "text",
          placeholder: "请选择${title}",
          options:  [{
            title: "文本框",
            value: "text"
          },
            {
              title: "日期(yyyy-MM-dd)",
              value: "date"
            },
            {
              title: "日期（yyyy-MM-dd HH:mm:ss）",
              value: "datetime"
            },
            {
              title: "时间（HH:mm:ss）",
              value: "time"
            },
            {
              title: "下拉框",
              value: "list"
            },
            {
              title: "下拉多选框",
              value: "list_multi"
            },
            {
              title: "下拉搜索框",
              value: "sel_search"
            },
            {
              title: "分类字典树",
              value: "cat_tree"
            },
            {
              title: "Popup弹框",
              value: "popup"
            },
            {
              title: "部门选择",
              value: "sel_depart"
            },
            {
              title: "用户选择",
              value: "sel_user"
            },
            {
              title: "省市区组件",
              value: "pca"
            },
            {
              title: "自定义树控件",
              value: "sel_tree"
            }]
        },
        {
          title: "字典Table",
          key: "queryDictTable",
          width: "130px",
          type: JVXETypes.input_pop,
          defaultValue: ""
        },
        {
          title: "字典Code",
          key: "queryDictField",
          width: "130px",
          type: JVXETypes.input,
          defaultValue: ""
        },
        {
          title: "字典Text",
          key: "queryDictText",
          width: "130px",
          type: JVXETypes.input,
          defaultValue: ""
        },
        {
          title: "默认值",
          key: "queryDefVal",
          width: "130px",
          type: JVXETypes.input,
          defaultValue: ""
        },
        {
          title: "是否启用",
          key: "queryConfigFlag",
          width: "80px",
          type: JVXETypes.checkbox,
          customValue: ["1", "0"],
          defaultChecked: !1
        }],
      isTrue1:false,
      isTrue2:false,
      isTrue3:false,
      form: this.$form.createForm(this),
      title: "操作",
      width: 1200,
      visible: false,
      model: {},
      labelCol: {
        xs: {span: 24},
        sm: {span: 5},
      },
      wrapperCol: {
        xs: {span: 24},
        sm: {span: 16},
      },
      confirmLoading: false,
      validatorRules: {},
      url: {
        add: "/online/cgform/api/addAll",
        edit:'/online/cgform/api/editAll',
        fieldUrl:'online/cgform/field/listByHeadId',
        yanzheng:'/online/cgform/api/checkOnlyTable',
        indexs:'/online/cgform/index/listByHeadId'
      },
      indexs:[],
      fields:[{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "id",
        dbFieldTxt: "主键",
        dbIsKey: "1",
        dbIsNull: "0",
        dbLength: "36",
        dbPointLength: "0",
        dbType: "string",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "text",
        isQuery: "0",
        isReadOnly: "1",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 1,
        order_num: 0,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
        disabled:true
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "create_by",
        dbFieldTxt: "创建人",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "50",
        dbPointLength: "0",
        dbType: "string",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "text",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 2,
        order_num: 1,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "create_time",
        dbFieldTxt: "创建日期",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "20",
        dbPointLength: "0",
        dbType: "Date",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "datetime",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 3,
        order_num: 2,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "update_by",
        dbFieldTxt: "更新人",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "50",
        dbPointLength: "0",
        dbType: "string",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "text",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 4,
        order_num: 3,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "update_time",
        dbFieldTxt: "更新日期",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "20",
        dbPointLength: "0",
        dbType: "Date",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "datetime",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 5,
        order_num: 4,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "sys_org_code",
        dbFieldTxt: "所属部门",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "64",
        dbPointLength: "0",
        dbType: "string",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "text",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 6,
        order_num: 5,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "parent_id",
        dbFieldTxt: "所属部门",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "64",
        dbPointLength: "0",
        dbType: "string",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "text",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 6,
        order_num: 5,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "order_num",
        dbFieldTxt: "所属部门",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "64",
        dbPointLength: "0",
        dbType: "string",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "text",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 6,
        order_num: 5,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      }]
    }
  },
  created() {
  },
  methods: {
    // 选择表的类型
    handleChange(value) {
      if(value == 2) {
        this.isMain = true
      }else {
        this.isMain = false
      }
    },
    compareToFirstPassword(rule, value, callback) {
      var httpurl = this.url.yanzheng
      var obj = {
        tbname: value,
        id:this.id||''
      }
      if(value == '') {
        callback('请输入表名!');
      }else{
        getAction(httpurl, obj).then((res) => {
          var data = res
          if(data.result==1) {
            callback();
          }else {
            callback('数据库已有此字段，请不要重复添加!');
          }
        }).finally(() => {
        })
      }

    },
    saveEditindex(row) {
      var id = row.orderNum-1
      this.indexs[id] = row
    },
    // 保存数据
    saveEdit(row) {
      var id = row.orderNum-1
      this.fields[id] = row
      this.columns5[1].options=[]
      for(var i = 0 ;i < this.fields.length;i++) {
        this.columns5[1].options.push({
          title:this.fields[i].dbFieldName,
          value:this.fields[i].dbFieldName
        })
      }
    },
    // 删除选中的rows
    remove() {
      var arr = this.selectedRowIds
      arr.sort(function(a,b){
        return  b - a;
      })
      for(var i = 0 ;  i < arr.length ; i++) {
        this.fields.splice(arr[i]-1,1)
      }
      this.selectedRowIds = []
    },
    // 获取选中的row
    selectRows(rows) {
      var arr =  []
      for(var i = 0 ; i < rows.length ;  i++) {
        arr.push(rows[i].orderNum)
      }
      this.selectedRowIds = arr
    },
    selfaddindex() {
      this.indexs.push({
        indexField: "",
        indexName: "",
        indexType: "normal",
      })
    },
    selfadd() {
      this.fields.push({
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "",
        dbFieldTxt: "",
        dbIsKey: "1",
        dbIsNull: "0",
        dbLength: "36",
        dbPointLength: "0",
        dbType: "string",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "text",
        isQuery: "0",
        isReadOnly: "1",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 1,
        order_num: 0,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      })
    },
    callback(key) {
      console.log(key);
    },
    // 呼出时间
    onChange1(date, dateString) {
      console.log(dateString)
    },
    onChange2(date, dateString) {
      console.log(dateString)
    },
    add() {
      this.id = ''
      this.edit({
        formCategory:"temp",
        formTemplate:'1',
        isCheckbox:'Y',
        isDesForm:'N',
        isPage:'Y',
        isTree:'N',
        scroll:1,
        tableName: '',
        tableTxt:'',
        tableType:2,
        idType:"UUID",
        queryMode:'single',
      });
    },
    edit(record) {
      console.log(record)
      var obj = {
        headId:record.id,
      }
      this.id = record.id
      if(record.id) {
        // 获取下面的表单
        getAction(this.url.fieldUrl, obj).then((res) => {
          console.log(res)
          if (res.success) {
            this.fields = res.result
          } else {
            this.$message.warning(res.message);
          }
        }).finally(() => {
        })
        getAction(this.url.indexs, obj).then((res) => {
          console.log(res)
          if (res.success) {
            this.indexs = res.result
          } else {
            this.$message.warning(res.message);
          }
        }).finally(() => {
        })
      }
      // for(var i = 0 ;i < this.fields.length;i++) {
      //   this.columns5[1].options.push({
      //     title:this.fields[i].dbFieldName,
      //     value:this.fields[i].dbFieldName
      //   })
      // }
      record.taskId = this.taskId;
      this.taskId =""
      this.form.resetFields();
      this.model = Object.assign({}, record);
      this.visible = true;
      this.$nextTick(() => {
        this.form.setFieldsValue(pick(this.model,'idType','queryMode','formCategory','formTemplate','isCheckbox','isDesForm','isPage','isTree','scroll','tableName','tableTxt','tableType'))
      })
    },
    close() {
      this.indexs = []
      this.fields = [{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "id",
        dbFieldTxt: "主键",
        dbIsKey: "1",
        dbIsNull: "0",
        dbLength: "36",
        dbPointLength: "0",
        dbType: "string",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "text",
        isQuery: "0",
        isReadOnly: "1",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 1,
        order_num: 0,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "create_by",
        dbFieldTxt: "创建人",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "50",
        dbPointLength: "0",
        dbType: "string",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "text",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 2,
        order_num: 1,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "create_time",
        dbFieldTxt: "创建日期",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "20",
        dbPointLength: "0",
        dbType: "Date",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "datetime",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 3,
        order_num: 2,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "update_by",
        dbFieldTxt: "更新人",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "50",
        dbPointLength: "0",
        dbType: "string",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "text",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 4,
        order_num: 3,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "update_time",
        dbFieldTxt: "更新日期",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "20",
        dbPointLength: "0",
        dbType: "Date",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "datetime",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 5,
        order_num: 4,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "sys_org_code",
        dbFieldTxt: "所属部门",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "64",
        dbPointLength: "0",
        dbType: "string",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "text",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 6,
        order_num: 5,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "parent_id",
        dbFieldTxt: "所属部门",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "64",
        dbPointLength: "0",
        dbType: "string",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "text",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 6,
        order_num: 5,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      },{
        converter: "",
        dbDefaultVal: "",
        dbFieldName: "order_num",
        dbFieldTxt: "所属部门",
        dbIsKey: "0",
        dbIsNull: "1",
        dbLength: "64",
        dbPointLength: "0",
        dbType: "string",
        dictField: "",
        dictTable: "",
        dictText: "",
        fieldDefaultValue: "",
        fieldExtendJson: "",
        fieldHref: "",
        fieldLength: "120",
        fieldMustInput: "0",
        fieldShowType: "text",
        isQuery: "0",
        isReadOnly: "0",
        isShowForm: "0",
        isShowList: "0",
        mainField: "",
        mainTable: "",
        orderNum: 6,
        order_num: 5,
        queryConfigFlag: "0",
        queryDefVal: "",
        queryDictField: "",
        queryDictTable: "",
        queryDictText: "",
        queryMode: "single",
        sortFlag: "0",
      }]
      this.$emit('close');
      this.visible = false;
    },
    ok1(res) {
      this.isTrue1 = res
    },
    ok2(res) {
      this.isTrue2 = res
    },
    ok3(res) {
      this.isTrue3 = res
    },
    handleOk() {
      const that = this;
      // 触发表单验证
      this.form.validateFields((err, values) => {
        if (!err) {
          this.$refs.demo1.validAllEvent()
          // this.$refs.demo2.validAllEvent()
          if(this.indexs.length!=0) {
            this.$refs.demo5.validAllEvent()
          }
          setTimeout(function() {
            if(that.isTrue1) {
              that.confirmLoading = true;
              let httpurl = '';
              let method = '';
              if (!that.model.id) {
                httpurl += that.url.add;
                method = 'post';
              } else {
                httpurl += that.url.edit;
                method = 'put';
              }
              let formData = Object.assign(that.model, values);
              console.log("表单提交数据", formData)
              var obj = {
                deleteFieldIds:[],
                deleteIndexIds:[],
                indexs:that.indexs,
                head:Object.assign(that.model, values),
                fields: that.fields
              }
              httpAction(httpurl, obj, method).then((res) => {
                if (res.success) {
                  that.$message.success(res.message);
                  that.indexs = []
                  that.fields = [{
                    converter: "",
                    dbDefaultVal: "",
                    dbFieldName: "id",
                    dbFieldTxt: "主键",
                    dbIsKey: "1",
                    dbIsNull: "0",
                    dbLength: "36",
                    dbPointLength: "0",
                    dbType: "string",
                    dictField: "",
                    dictTable: "",
                    dictText: "",
                    fieldDefaultValue: "",
                    fieldExtendJson: "",
                    fieldHref: "",
                    fieldLength: "120",
                    fieldMustInput: "0",
                    fieldShowType: "text",
                    isQuery: "0",
                    isReadOnly: "1",
                    isShowForm: "0",
                    isShowList: "0",
                    mainField: "",
                    mainTable: "",
                    orderNum: 1,
                    order_num: 0,
                    queryConfigFlag: "0",
                    queryDefVal: "",
                    queryDictField: "",
                    queryDictTable: "",
                    queryDictText: "",
                    queryMode: "single",
                    sortFlag: "0",
                  },{
                    converter: "",
                    dbDefaultVal: "",
                    dbFieldName: "create_by",
                    dbFieldTxt: "创建人",
                    dbIsKey: "0",
                    dbIsNull: "1",
                    dbLength: "50",
                    dbPointLength: "0",
                    dbType: "string",
                    dictField: "",
                    dictTable: "",
                    dictText: "",
                    fieldDefaultValue: "",
                    fieldExtendJson: "",
                    fieldHref: "",
                    fieldLength: "120",
                    fieldMustInput: "0",
                    fieldShowType: "text",
                    isQuery: "0",
                    isReadOnly: "0",
                    isShowForm: "0",
                    isShowList: "0",
                    mainField: "",
                    mainTable: "",
                    orderNum: 2,
                    order_num: 1,
                    queryConfigFlag: "0",
                    queryDefVal: "",
                    queryDictField: "",
                    queryDictTable: "",
                    queryDictText: "",
                    queryMode: "single",
                    sortFlag: "0",
                  },{
                    converter: "",
                    dbDefaultVal: "",
                    dbFieldName: "create_time",
                    dbFieldTxt: "创建日期",
                    dbIsKey: "0",
                    dbIsNull: "1",
                    dbLength: "20",
                    dbPointLength: "0",
                    dbType: "Date",
                    dictField: "",
                    dictTable: "",
                    dictText: "",
                    fieldDefaultValue: "",
                    fieldExtendJson: "",
                    fieldHref: "",
                    fieldLength: "120",
                    fieldMustInput: "0",
                    fieldShowType: "datetime",
                    isQuery: "0",
                    isReadOnly: "0",
                    isShowForm: "0",
                    isShowList: "0",
                    mainField: "",
                    mainTable: "",
                    orderNum: 3,
                    order_num: 2,
                    queryConfigFlag: "0",
                    queryDefVal: "",
                    queryDictField: "",
                    queryDictTable: "",
                    queryDictText: "",
                    queryMode: "single",
                    sortFlag: "0",
                  },{
                    converter: "",
                    dbDefaultVal: "",
                    dbFieldName: "update_by",
                    dbFieldTxt: "更新人",
                    dbIsKey: "0",
                    dbIsNull: "1",
                    dbLength: "50",
                    dbPointLength: "0",
                    dbType: "string",
                    dictField: "",
                    dictTable: "",
                    dictText: "",
                    fieldDefaultValue: "",
                    fieldExtendJson: "",
                    fieldHref: "",
                    fieldLength: "120",
                    fieldMustInput: "0",
                    fieldShowType: "text",
                    isQuery: "0",
                    isReadOnly: "0",
                    isShowForm: "0",
                    isShowList: "0",
                    mainField: "",
                    mainTable: "",
                    orderNum: 4,
                    order_num: 3,
                    queryConfigFlag: "0",
                    queryDefVal: "",
                    queryDictField: "",
                    queryDictTable: "",
                    queryDictText: "",
                    queryMode: "single",
                    sortFlag: "0",
                  },{
                    converter: "",
                    dbDefaultVal: "",
                    dbFieldName: "update_time",
                    dbFieldTxt: "更新日期",
                    dbIsKey: "0",
                    dbIsNull: "1",
                    dbLength: "20",
                    dbPointLength: "0",
                    dbType: "Date",
                    dictField: "",
                    dictTable: "",
                    dictText: "",
                    fieldDefaultValue: "",
                    fieldExtendJson: "",
                    fieldHref: "",
                    fieldLength: "120",
                    fieldMustInput: "0",
                    fieldShowType: "datetime",
                    isQuery: "0",
                    isReadOnly: "0",
                    isShowForm: "0",
                    isShowList: "0",
                    mainField: "",
                    mainTable: "",
                    orderNum: 5,
                    order_num: 4,
                    queryConfigFlag: "0",
                    queryDefVal: "",
                    queryDictField: "",
                    queryDictTable: "",
                    queryDictText: "",
                    queryMode: "single",
                    sortFlag: "0",
                  },{
                    converter: "",
                    dbDefaultVal: "",
                    dbFieldName: "sys_org_code",
                    dbFieldTxt: "所属部门",
                    dbIsKey: "0",
                    dbIsNull: "1",
                    dbLength: "64",
                    dbPointLength: "0",
                    dbType: "string",
                    dictField: "",
                    dictTable: "",
                    dictText: "",
                    fieldDefaultValue: "",
                    fieldExtendJson: "",
                    fieldHref: "",
                    fieldLength: "120",
                    fieldMustInput: "0",
                    fieldShowType: "text",
                    isQuery: "0",
                    isReadOnly: "0",
                    isShowForm: "0",
                    isShowList: "0",
                    mainField: "",
                    mainTable: "",
                    orderNum: 6,
                    order_num: 5,
                    queryConfigFlag: "0",
                    queryDefVal: "",
                    queryDictField: "",
                    queryDictTable: "",
                    queryDictText: "",
                    queryMode: "single",
                    sortFlag: "0",
                  }]
                  that.$emit('ok');
                } else {
                  that.$message.warning(res.message);
                }
              }).finally(() => {
                that.confirmLoading = false;
                that.indexs = []
                that.fields = [{
                  converter: "",
                  dbDefaultVal: "",
                  dbFieldName: "id",
                  dbFieldTxt: "主键",
                  dbIsKey: "1",
                  dbIsNull: "0",
                  dbLength: "36",
                  dbPointLength: "0",
                  dbType: "string",
                  dictField: "",
                  dictTable: "",
                  dictText: "",
                  fieldDefaultValue: "",
                  fieldExtendJson: "",
                  fieldHref: "",
                  fieldLength: "120",
                  fieldMustInput: "0",
                  fieldShowType: "text",
                  isQuery: "0",
                  isReadOnly: "1",
                  isShowForm: "0",
                  isShowList: "0",
                  mainField: "",
                  mainTable: "",
                  orderNum: 1,
                  order_num: 0,
                  queryConfigFlag: "0",
                  queryDefVal: "",
                  queryDictField: "",
                  queryDictTable: "",
                  queryDictText: "",
                  queryMode: "single",
                  sortFlag: "0",
                },{
                  converter: "",
                  dbDefaultVal: "",
                  dbFieldName: "create_by",
                  dbFieldTxt: "创建人",
                  dbIsKey: "0",
                  dbIsNull: "1",
                  dbLength: "50",
                  dbPointLength: "0",
                  dbType: "string",
                  dictField: "",
                  dictTable: "",
                  dictText: "",
                  fieldDefaultValue: "",
                  fieldExtendJson: "",
                  fieldHref: "",
                  fieldLength: "120",
                  fieldMustInput: "0",
                  fieldShowType: "text",
                  isQuery: "0",
                  isReadOnly: "0",
                  isShowForm: "0",
                  isShowList: "0",
                  mainField: "",
                  mainTable: "",
                  orderNum: 2,
                  order_num: 1,
                  queryConfigFlag: "0",
                  queryDefVal: "",
                  queryDictField: "",
                  queryDictTable: "",
                  queryDictText: "",
                  queryMode: "single",
                  sortFlag: "0",
                },{
                  converter: "",
                  dbDefaultVal: "",
                  dbFieldName: "create_time",
                  dbFieldTxt: "创建日期",
                  dbIsKey: "0",
                  dbIsNull: "1",
                  dbLength: "20",
                  dbPointLength: "0",
                  dbType: "Date",
                  dictField: "",
                  dictTable: "",
                  dictText: "",
                  fieldDefaultValue: "",
                  fieldExtendJson: "",
                  fieldHref: "",
                  fieldLength: "120",
                  fieldMustInput: "0",
                  fieldShowType: "datetime",
                  isQuery: "0",
                  isReadOnly: "0",
                  isShowForm: "0",
                  isShowList: "0",
                  mainField: "",
                  mainTable: "",
                  orderNum: 3,
                  order_num: 2,
                  queryConfigFlag: "0",
                  queryDefVal: "",
                  queryDictField: "",
                  queryDictTable: "",
                  queryDictText: "",
                  queryMode: "single",
                  sortFlag: "0",
                },{
                  converter: "",
                  dbDefaultVal: "",
                  dbFieldName: "update_by",
                  dbFieldTxt: "更新人",
                  dbIsKey: "0",
                  dbIsNull: "1",
                  dbLength: "50",
                  dbPointLength: "0",
                  dbType: "string",
                  dictField: "",
                  dictTable: "",
                  dictText: "",
                  fieldDefaultValue: "",
                  fieldExtendJson: "",
                  fieldHref: "",
                  fieldLength: "120",
                  fieldMustInput: "0",
                  fieldShowType: "text",
                  isQuery: "0",
                  isReadOnly: "0",
                  isShowForm: "0",
                  isShowList: "0",
                  mainField: "",
                  mainTable: "",
                  orderNum: 4,
                  order_num: 3,
                  queryConfigFlag: "0",
                  queryDefVal: "",
                  queryDictField: "",
                  queryDictTable: "",
                  queryDictText: "",
                  queryMode: "single",
                  sortFlag: "0",
                },{
                  converter: "",
                  dbDefaultVal: "",
                  dbFieldName: "update_time",
                  dbFieldTxt: "更新日期",
                  dbIsKey: "0",
                  dbIsNull: "1",
                  dbLength: "20",
                  dbPointLength: "0",
                  dbType: "Date",
                  dictField: "",
                  dictTable: "",
                  dictText: "",
                  fieldDefaultValue: "",
                  fieldExtendJson: "",
                  fieldHref: "",
                  fieldLength: "120",
                  fieldMustInput: "0",
                  fieldShowType: "datetime",
                  isQuery: "0",
                  isReadOnly: "0",
                  isShowForm: "0",
                  isShowList: "0",
                  mainField: "",
                  mainTable: "",
                  orderNum: 5,
                  order_num: 4,
                  queryConfigFlag: "0",
                  queryDefVal: "",
                  queryDictField: "",
                  queryDictTable: "",
                  queryDictText: "",
                  queryMode: "single",
                  sortFlag: "0",
                },{
                  converter: "",
                  dbDefaultVal: "",
                  dbFieldName: "sys_org_code",
                  dbFieldTxt: "所属部门",
                  dbIsKey: "0",
                  dbIsNull: "1",
                  dbLength: "64",
                  dbPointLength: "0",
                  dbType: "string",
                  dictField: "",
                  dictTable: "",
                  dictText: "",
                  fieldDefaultValue: "",
                  fieldExtendJson: "",
                  fieldHref: "",
                  fieldLength: "120",
                  fieldMustInput: "0",
                  fieldShowType: "text",
                  isQuery: "0",
                  isReadOnly: "0",
                  isShowForm: "0",
                  isShowList: "0",
                  mainField: "",
                  mainTable: "",
                  orderNum: 6,
                  order_num: 5,
                  queryConfigFlag: "0",
                  queryDefVal: "",
                  queryDictField: "",
                  queryDictTable: "",
                  queryDictText: "",
                  queryMode: "single",
                  sortFlag: "0",
                }]
                that.close();
              })
            }
          },100)


        }

      })
    },
    handleCancel() {
      this.close()
    },
    popupCallback(row) {
      this.form.setFieldsValue(pick(row, 'name'))
    },

  }
}
</script>
<style scoped>
.form-item {
  margin-left: 15px
}

/deep/ .ant-form .ant-form-item {
  margin-bottom: 0;
}

/deep/ .form-item {
  margin-left: 0;
}

/deep/ .ant-form > .ant-row {
  border-bottom: 1px solid #e8e8e8;
  padding: 12px 0;
}

/deep/ .ant-form label {
  font-size: 12px;
}
</style>