<template>
  <el-table :data="tableData" style="width: 100%;padding-top: 15px;">
    <el-table-column width="200">
    </el-table-column>
    <el-table-column label="正在进行的考试" min-width="200" width="350">
    </el-table-column>
    <el-table-column label="考试时间" width="400" align="center">
    </el-table-column>
    <el-table-column label="未完成人数" width="250" align="center">
    </el-table-column>
    <el-table-column label="操作" align="center">
    </el-table-column>
  </el-table>
</template>

<script>
import { transactionList } from '@/api/remote-search'

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        success: 'success',
        pending: 'danger'
      }
      return statusMap[status]
    },
    orderNoFilter(str) {
      return str.substring(0, 30)
    }
  },
  data() {
    return {
      list: null
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      transactionList().then(response => {
        this.list = response.data.items.slice(0, 8)
      })
    }
  }
}
</script>
