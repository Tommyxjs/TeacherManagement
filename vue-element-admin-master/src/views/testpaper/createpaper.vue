<template>
  <div style="margin-top: 2%; margin-left: 3%;">
  <el-row :gutter="12">
     <el-tabs type="border-card" style="width: 95%;">
       <el-tab-pane label="固定组卷">
         <el-input v-model="filename" placeholder="请输入搜索内容" style="width:350px;margin-top: 1%;display: inline-block;" prefix-icon="el-icon-document" />


  <el-table
      style="margin-top: 2%;"
      ref="multipleTable"
      v-loading="listLoading"
      :data="list"
      element-loading-text="拼命加载中"
      border
      fit
      highlight-current-row
      @selection-change="handleSelectionChange"
    >
      <el-table-column type="selection" align="center" />
      <el-table-column align="center" label="序号" width="95">
        <template slot-scope="scope">
          {{ scope.$index }}
        </template>
      </el-table-column>
      <el-table-column label="题目">
        <template slot-scope="scope">
          {{ scope.row.title }}
        </template>
      </el-table-column>
      <el-table-column label="分类">
        <template slot-scope="scope">
          {{ scope.row.title }}
        </template>
      </el-table-column>
      <el-table-column label="题型">
        <template slot-scope="scope">
          {{ scope.row.title }}
        </template>
      </el-table-column>
      <el-table-column label="作者" width="110" align="center">
        <template slot-scope="scope">
          <el-tag>{{ scope.row.author }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column label="分数" width="115" align="center">
        <template slot-scope="scope">
          {{ scope.row.pageviews }}
        </template>
      </el-table-column>
      <el-table-column align="center" label="操作" width="110">
     <template slot-scope="{row}">
       <el-button
         v-if="row.edit"
         type="success"
         size="small"
         icon="el-icon-circle-check-outline"
         @click="confirmEdit(row)"
       >
         Ok
       </el-button>
       <el-button
         v-else
         type="primary"
         size="small"
         icon="el-icon-edit"
         @click="row.edit=!row.edit"
       >
         Edit
       </el-button>
     </template>
      </el-table-column>
    </el-table>
  <el-button type="primary" round style="float: right;margin-right: 50px;margin-top: 2%;"><i class="el-icon-circle-check"></i>&nbsp;保存试题</el-button>



       </el-tab-pane>
       <el-tab-pane label="随机组卷">
         <i>注意：使用随机组卷后，每个考生的试卷均不一致</i></br>
          <el-button type="primary" plain style="margin-top: 2%;">选择分类</el-button></br>
            <el-radio-group v-model="radio" style="margin-top: 3%;">
              <el-radio :label="3">随机顺序</el-radio>
              <el-radio :label="6">题型分类随机</el-radio>
              <el-radio :label="9">分数排序</el-radio>
            </el-radio-group></br>
            <el-button type="primary" round style="margin-top: 3%;"><i class="el-icon-circle-check"></i>&nbsp;保存试题</el-button>
        </el-tab-pane>
     </el-tabs>
  </el-row>
  </div>
</template>

<script>
  export default {
    data() {
      return {

      };
    },
    methods: {

    }
  }
</script>

<style>
</style>
