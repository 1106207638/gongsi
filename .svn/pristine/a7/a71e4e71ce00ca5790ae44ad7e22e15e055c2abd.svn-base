<template>
  <div>
    <a-modal
        title="代码生成"
        :visible="visible"
        :width="width"
        :confirm-loading="confirmLoading"
        @ok="handleOk"
        @cancel="handleCancel"
        okText="下载到本地"
    >
      <div v-for="(item,index) in list" :key="index">
        {{item}}
      </div>
    </a-modal>
  </div>
</template>
<script>
import {getAction, httpAction} from '@/api/manage'

export default {
  data() {
    return {
      width:1200,
      ModalText: 'Content of the modal',
      visible: false,
      confirmLoading: false,
      list:[],
      url:{
        down:'http://192.168.1.123:8080/jeecg-boot/online/cgform/api/downGenerateCode'
      }
    };
  },
  methods: {
    showModal() {
      this.visible = true;
    },
    add(record) {
      this.visible = true
      this.list = record
    },
    handleOk(e) {
      var httpurl = this.url.down+'?fileList='+this.list.toString()
      // var httpurl = this.url.down
      // var obj = {
      //   fileList:this.list.toString()
      // }
      var xmlreq = new XMLHttpRequest();
      xmlreq.open('get', httpurl, true)
      xmlreq.responseType = 'blob';
      xmlreq.setRequestHeader('Content-Type', 'application/json');
      xmlreq.setRequestHeader("X-Access-Token", JSON.parse(window.localStorage.getItem('pro__Access-Token')).value);
      xmlreq.send()

      xmlreq.onreadystatechange = function () {
        // 为了保证 数据 完整返回，我们一般会判断 两个值
        if (xmlreq.readyState == 4 && xmlreq.status == 200) {
          xmlreq.onload = function () {
            var data = xmlreq.response;
            var blob = new Blob([data]);
            var a = document.createElement('a');
            var blobUrl = window.URL.createObjectURL(blob);
            a.download = '代码生成.zip'
            a.href = blobUrl;
            a.click();
          };
        } else if (xmlreq.status != 200) {

        }
      }

      // getAction(httpurl, obj).then((res) => {
      //   console.log(res)
      //     var data = res
      //     var blob = new Blob([data]);
      //     var a = document.createElement('a');
      //     var blobUrl = window.URL.createObjectURL(blob);
      //     a.download = '生成代码.zip'
      //     a.href = blobUrl;
      //     a.click();
      //
      //
      //
      //
      //
      // }).finally(() => {
      // })
    },
    handleCancel(e) {
      console.log('Clicked cancel button');
      this.visible = false;
    },
  },
};
</script>