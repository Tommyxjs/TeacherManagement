<template>
  <div class="app-container">
    <upload-excel-component :on-success="handleSuccess" :before-upload="beforeUpload" />
    <el-button style="margin-bottom:20px;margin-right: 1%;" type="primary" icon="el-icon-document" @click="handleUpload">
      确认导入
    </el-button>
    <el-table ref="multipleTable" :data="tableData" border highlight-current-row style="width: 100%;margin-top:20px;" @selection-change="handleSelectionChange">
      <el-table-column type="selection" align="center" />
      <el-table-column v-for="item of tableHeader" :key="item" :prop="item" :label="item" />
    </el-table>
  </div>
</template>

<script>
import UploadExcelComponent from '@/components/UploadExcel/index.vue'

export default {
  name: 'UploadExcel',
  components: { UploadExcelComponent },
  data() {
    return {
      tableData: [],
      tableHeader: [],
      multipleSelection: [],
    }
  },
  methods: {
    beforeUpload(file) {
      const isLt1M = file.size / 1024 / 1024 < 1

      if (isLt1M) {
        return true
      }

      this.$message({
        message: 'Please do not upload files larger than 1m in size.',
        type: 'warning'
      })
      return false
    },
    handleUpload(){
      console.log(this.multipleSelection);
      const h = this.$createElement;
      //JSON.parse(JSON.stringify(this.multipleSelection).replace(/'序号'/g, 'questionsId'))
      //console.log(this.multipleSelection);

      this.$axios({
        url:'http://172.19.18.20:8001/questions/info/',
        method:'post',
        params:{},
        data:{multipleSelection:this.multipleSelection},
      }).then((response)=>{
        this.$notify({
          title: '导入提示',
          message: h('i', { style: 'color: blue'}, '导入成功'),
          duration: 2500
        });
      }).catch(error=>{
        this.$notify({
          title: '导入提示',
          message: h('i', { style: 'color: red'}, '导入失败'),
          duration: 2500
        });
        console.log(error)
      })
    },
    handleSelectionChange(val) {
      this.multipleSelection = val
    },
    handleSuccess({ results, header }) {
      this.tableData = results
      this.tableHeader = header
    }
  }
}
</script>
