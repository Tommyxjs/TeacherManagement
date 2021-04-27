<template>
  <div class="app-container documentation-container">
    <el-input v-model="input" placeholder="请输入内容" @change="search()" size="small" style="width: 300px"></el-input>
    <el-row>
   <el-button type="primary" round @click="handleCreate"><i class="el-icon-plus"></i>&nbsp;新增考生</el-button>
   <el-dialog :visible.sync="dialogFormVisible">
     <el-form
       :model="studentForm"
       label-position="left"
       label-width="90px"
       style="width: 400px; margin-left:50px;"
       status-icon :rules="rules"
     >
       <el-form-item label="ID" prop="stuId">
         <el-input  v-model="studentForm.stuId"></el-input>
       </el-form-item>
       <el-form-item label="姓名"  prop="stuName">
         <el-input v-model="studentForm.stuName" />
       </el-form-item>
       <el-form-item label="机构" prop="institution">
         <el-input v-model="studentForm.institution" />
       </el-form-item>
       <el-form-item label="电话号码" prop="phone">
         <el-input   v-model="studentForm.phone" />
       </el-form-item>
       <el-form-item label="身份证号码" prop="cidNum">
         <el-input  v-model="studentForm.cidNum" />
       </el-form-item>
     </el-form>
     <div slot="footer" class="dialog-footer">
       <el-button @click="dialogFormVisible = false">取消</el-button>
       <el-button type="primary" @click=" submitData() ">确定</el-button>
     </div>
   </el-dialog>
   <el-dialog :visible.sync="dialogFormVisible1">
     <el-form
       :model="studentForm"
       label-position="left"
       label-width="90px"
       style="width: 400px; margin-left:50px;"
       status-icon :rules="rules"
     >
       <el-form-item label="ID" prop="stuId" >
         <el-input  v-model="studentForm.stuId" :disabled="true"></el-input>
       </el-form-item>
       <el-form-item label="姓名"  prop="stuName">
         <el-input v-model="studentForm.stuName" />
       </el-form-item>
       <el-form-item label="机构" prop="institution">
         <el-input v-model="studentForm.institution" :disabled="true" />
       </el-form-item>
       <el-form-item label="电话号码" prop="phone">
         <el-input   v-model="studentForm.phone" />
       </el-form-item>
       <el-form-item label="身份证号码" prop="cidNum">
         <el-input  v-model="studentForm.cidNum" />
       </el-form-item>
     </el-form>
     <div slot="footer" class="dialog-footer">
       <el-button @click="dialogFormVisible1 = false">取消</el-button>
       <el-button type="primary" @click=" submitData1() ">确定</el-button>
     </div>
   </el-dialog>
   <el-button type="danger" @click="deleteStudent()" round style="margin-left: 35px;"><i class="el-icon-delete"></i>&nbsp;删除考生</el-button>
   </el-row>

   <el-row style="display: flex;">
   <el-card class="el-card">
     <div>
        本周报考人数
     </div>
     <div style="color: #ffaa00;font-size: 35px;">
        0
     </div>
   </el-card>
   <el-card class="el-card">
     <div>
        已录入人数
     </div>
     <div style="color: #ffaa00;font-size: 35px;">
        0
     </div>
   </el-card>
   <el-card class="el-card">
     <div>
        未录入人数
     </div>
     <div style="color: #ffaa00;font-size: 35px;">
        0
     </div>
   </el-card>
    </el-row>
    <el-row>
      <el-table ref="multipleTable" :data="list" border fit highlight-current-row style="width: 100%;margin-top: 2%;" @selection-change="handleSelectionChange">
        <el-table-column type="selection" align="center" />

        <el-table-column align="center" label="学生ID" width="160">
          <template slot-scope="scope">
            <span>{{ scope.row.stuId }}</span>
          </template>
        </el-table-column>

        <el-table-column width="120px"  align="center" label="学生姓名">
          <template slot-scope="scope">
            <span>{{ scope.row.stuName }}</span>
          </template>
        </el-table-column>
        <el-table-column class-name="status-col" label="手机号码" width="160">
          <template slot-scope="scope">
            <span>{{ scope.row.phone }}</span>
          </template>
        </el-table-column>

        <el-table-column min-width="300px" align="center"  label="机构">
          <template slot-scope="scope">
            <span>{{ scope.row.institution }}</span>
            </template>
        </el-table-column>

        <el-table-column align="center" label="操作" width="120">
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
              @click="edit(row)"
            >
              Edit
            </el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-row>
    <el-pagination align='center'
  :page-size="10"
  :current-page.sync="page"
  @current-change="givepage()"
  layout="prev, pager, next"
  :total=total>
</el-pagination>
  </div>
</template>

<script>
import { fetchList } from '@/api/article'
import qs from 'qs';
export default {
  data() {
    var checkId= (rule, value, callback) => {
      var reg = /^[0-9]+.?[0-9]*$/;
      if (!reg.test(value)) {
         return callback(new Error('ID必须为数字'));
      }
    };
    var checkInstitution= (rule, value, callback) => {
      var newvalue = value.replace(/[^\x00-\xff]/g, "**");
      var length = newvalue.length;
      if (!value) {
        return callback(new Error('机构不能为空'));
      }
      else if(length>16){
        return callback(new Error('超过字数限制'));
      }

    };
    var checkPhoneNumber= (rule, value, callback) => {
      var newvalue = value.replace(/[^\x00-\xff]/g, "**");
      var reg = /^[0-9]+.?[0-9]*$/;
      if (!reg.test(value)) {
         return callback(new Error('电话必须为数字'));
      }
      var length = newvalue.length;
      if (!value) {
        return callback(new Error('电话号码不能为空'));
      }
      else if(length>64){
        return callback(new Error('超过字数限制'));
      }
    };
    var checkCidNum = (rule, value, callback) => {
      var newvalue = value.replace(/[^\x00-\xff]/g, "**");
      var reg = /^[0-9]+.?[0-9]*$/;
      if (!reg.test(value)) {
         return callback(new Error('电话必须为数字'));
      }
      var length = newvalue.length;
      if (!value) {
        return callback(new Error('电话号码不能为空'));
      }
      else if(length>64){
        return callback(new Error('超过字数限制'));
      }
    };
    var checkName= (rule, value, callback) => {
      var newvalue = value.replace(/[^\x00-\xff]/g, "**");
      var length = newvalue.length;
      if (!value) {
        return callback(new Error('姓名不能为空'));
      }
      else if(length>16){
        return callback(new Error('超过字数限制'));
      }
    };
    return {
      list: null,
      listLoading: true,
      multipleSelection: [],
      downloadLoading: false,
      filename: '',
      input: '',
          list: null,
          page: 1,
          total:0,

      studentForm: {
        institution: "",
        stuId:"",
        stuName: "",
        phone: "",
        cidNum:"",
      },
      dialogFormVisible: false,
      dialogFormVisible1:false,

      rules:{
        institution: [{validator: checkInstitution,trigger:'blur'}],
        stuId: [{validator: checkId,trigger:'blur'}],
        stuName: [{validator: checkName,trigger:'blur'}],
        phone: [{validator: checkPhoneNumber,trigger:'blur'}],
        cidNum: [{validator: checkCidNum,trigger:'blur'}],
      }
    }

  },
  methods:{
    handleCreate() {
      this.dialogFormVisible = true;
      //console.log(this.dialogFormVisible);
      this.studentForm = {
        institution: "",
        stuId:"",
        stuName: "",
        phoneNumber: "",
        cidNum:"",

      };
      //this.dialogFormVisible = true;
    },
    submitData(){
      console.log("22222");
      this.$axios({
        url:'http://172.19.18.20:8001/stu/info/',
        method:'post',
        params:{},
        data:{institution:this.studentForm.institution,stuId:this.studentForm.stuId,cidNum:this.studentForm.cidNum,stuName:this.studentForm.stuName,phone:this.studentForm.phone},
      }).then((response)=>{
        this.dialogFormVisible = false;
        this.fetchData();
      }).catch(error=>{
        console.log(error)
      })
    },
    submitData1(){
      console.log(this.studentForm);
      const h = this.$createElement;

      this.$axios({
        url:'http://172.19.18.20:8001/stu/info/',
        method:'put',
        data:{institution:this.studentForm.institution,stuId:this.studentForm.stuId,cidNum:this.studentForm.cidNum,stuName:this.studentForm.stuName,phone:this.studentForm.phone},
        params:{},
      }).then((response)=>{

        this.$notify({
          title: '修改提示',
          message: h('i', { style: 'color: blue'}, '修改成功'),
          duration: 2500
        });
        this.fetchData();
        console.log("success");
        this.dialogFormVisible1 = false;
      }).catch(error=>{
        this.$notify({
          title: '修改提示',
          message: h('i', { style: 'color: red'}, '修改失败'),
          duration: 2500
        });
        this.fetchData();
        console.log(error)
      })
    },
    deleteStudent(){
      var deleteSelection = [].concat(this.multipleSelection);
      console.log(deleteSelection);
      var deleteStuId=[];
      var deleteInstitution=[];
      for(var i=0,len=this.multipleSelection.length; i<len; i++){
        console.log(this.multipleSelection[i].stuId);
        deleteStuId[i]=this.multipleSelection[i].stuId;
        deleteInstitution[i]=this.multipleSelection[i].institution;
      }
      const h = this.$createElement;
      console.log(this.multipleSelection);
      console.log(deleteStuId);
      console.log(deleteInstitution);
      //for (var i=0,len=this.multipleSelection.length; i<len; i++){
        //console.log(this.multipleSelection[i].institution);
        //console.log(this.multipleSelection[i].questionsId);
      this.$axios({
        url:'http://172.19.18.20:8001/stu/info/',
        method:'delete',
        data:{},
        params:{'deleteInstitution':deleteInstitution,'deleteStuId':deleteStuId},
    // 重点在这里
    paramsSerializer: params => {
      console.log('transport')
      return qs.stringify(params, { arrayFormat: 'repeat' })
    }
      }).then((response)=>{
        this.$notify({
          title: '删除提示',
          message: h('i', { style: 'color: blue'}, '删除成功'),
          duration: 2500
        });
        this.fetchData();
      }).catch(error=>{
        this.$notify({
          title: '删除提示',
          message: h('i', { style: 'color: red'}, '删除失败'),
          duration: 2500
        });
        console.log(error)
      })
    //}
      //console.log(this.multipleSelection)
    },
    edit(row) {
      this.studentForm=row;
      this.dialogFormVisible1 = true;

      //row.edit=!row.edit;
    },
    handleSelectionChange(val) {
      this.multipleSelection = val
    },
    search(){
      console.log(this.page);
      this.$axios({
          url:'http://172.19.18.20:8001/stu/info/',
          method:'get',
          params:{'page':1,'input':this.input},
          data:'',
        }).then((response)=>{
          this.list=response.data.data;
          this.total=response.data.total;
          this.page=1;
        }).catch(error=>{
          console.log('response error');
        });
    },

    givepage(){
      console.log(this.page);
      this.$axios({
          url:'http://172.19.18.20:8001/stu/info/',
          method:'get',
          params:{'page':this.page,'input':this.input},
          data:'',
        }).then((response)=>{
          this.list=response.data.data;
          this.total=response.data.total;
        }).catch(error=>{
          console.log('response error');
        });
    },
    fetchData(){
      this.$axios({
            url:'http://172.19.18.20:8001/stu/info/',
            method:'get',
            params:{'page':this.page,'input':this.input},
            data:'',
          }).then((response)=>{
            this.list=response.data.data;
            this.total=response.data.total;
          }).catch(error=>{
            console.log('response error');
          });
    }
  },
  mounted(){
    this.fetchData();
    }
}
</script>

<style lang="scss" scoped>
.documentation-container {
  margin: 50px;
  flex-wrap: wrap;
  justify-content: flex-start;


  .el-card{
    margin-right: 2%;
    margin-top: 2%;
    width: 30%;
    text-align: center;
  }

  .document-btn {
    flex-shrink: 0;
    display: block;
    cursor: pointer;
    background: black;
    color: white;
    height: 60px;
    padding: 0 16px;
    margin: 16px;
    line-height: 60px;
    font-size: 20px;
    text-align: center;
  }
}
</style>
