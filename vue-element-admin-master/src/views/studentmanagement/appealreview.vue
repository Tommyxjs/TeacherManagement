<template>
  <div style="margin-top: 3%;margin-left: 2%;">
    <el-row style="display: flex;">
    <el-card class="el-card">
      <div>
         待处理人数
      </div>
      <div style="color: #ffaa00;font-size: 35px;">
         0
      </div>
    </el-card>
    <el-card class="el-card">
      <div>
         已处理人数
      </div>
      <div style="color: #ffaa00;font-size: 35px;">
         0
      </div>
    </el-card>
    <el-card class="el-card">
      <div>
         本周审核人数
      </div>
      <div style="color: #ffaa00;font-size: 35px;">
         0
      </div>
    </el-card>
     </el-row>
     </br> </br>
     <el-row>
    <el-timeline>
      <el-timeline-item timestamp="2018/4/3" placement="top">
        <el-card style="width: 55%;">
          <h4>申诉请求</h4>
          <p style="display: inline-block;">刘烨萌 提交于 2018/4/3 20:46</p>
          <el-button type="text" class="button" style="display: inline;float: right;margin-right: 30px;">点击审核</el-button>
        </el-card>
      </el-timeline-item>
      <el-timeline-item timestamp="2018/4/3" placement="top">
        <el-card style="width: 55%;">
          <h4>申诉请求</h4>
          <p style="display: inline-block;">徐健朔 提交于 2018/4/3 20:46</p>
          <el-button type="text" class="button" style="display: inline;float: right;margin-right: 30px;">点击审核</el-button>
        </el-card>
      </el-timeline-item>
      <el-timeline-item timestamp="2018/4/3" placement="top">
        <el-card style="width: 55%;">
          <h4>申诉请求</h4>
          <p style="display: inline-block;">万靖 提交于 2018/4/3 20:46</p>
          <el-button type="text" class="button" style="display: inline;float: right;margin-right: 30px;">点击审核</el-button>
        </el-card>
      </el-timeline-item>
      <el-timeline-item timestamp="2018/4/3" placement="top">
        <el-card style="width: 55%;">
          <h4>申诉请求</h4>
          <p style="display: inline-block;">陈翔泰 提交于 2018/4/3 20:46</p>
          <el-button type="text" class="button" style="display: inline;float: right;margin-right: 30px;">点击审核</el-button>
        </el-card>
      </el-timeline-item>
    </el-timeline>
     </el-row>
  </div>

</template>

<script>
</script>

<style scoped>

  .el-card{ 
    margin-right: 2%;
    margin-top: 2%;
    width: 30%;
    text-align: center;
  }
</style>
