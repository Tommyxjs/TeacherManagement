<template>
  <div class="app-container">
    <el-input v-model="input" placeholder="请输入内容" @change="search()" size="small" style="width: 300px"></el-input>
    <el-button type="primary" round style="margin-left: 45%;" @click="handleCreate"><i class="el-icon-plus"></i>&nbsp;新增试题

    </el-button>
    <el-dialog :visible.sync="dialogFormVisible">
      <el-form
        :model="questionForm"
        label-position="left"
        label-width="90px"
        style="width: 400px; margin-left:50px;"
        status-icon :rules="rules"
      >
        <el-form-item label="机构" prop="institution">
          <el-input  v-model="questionForm.institution"></el-input>
        </el-form-item>
        <el-form-item label="分类" prop="category">
    <el-select v-model="questionForm.category" placeholder="请选择分类">
      <el-option label="选择题" value="select"></el-option>
      <el-option label="填空题" value="fillout"></el-option>
      <el-option label="判断题" value="judge"></el-option>
      <el-option label="简答题" value="answer"></el-option>
    </el-select>
  </el-form-item>
        <el-form-item label="题目"  prop="question">
          <el-input type="textarea" v-model="questionForm.question" />
        </el-form-item>
        <el-form-item label="答案" prop="standardanswer">
          <el-input type="textarea" v-model="questionForm.standardanswer" />
        </el-form-item>
        <el-form-item label="分数" prop="score">
          <el-input type="number"  v-model="questionForm.score" />
        </el-form-item>
        <el-form-item label="难度" prop="level">
        <el-select v-model="questionForm.level" placeholder="请选择难度">
          <el-option label="容易" value="1"></el-option>
          <el-option label="偏易" value="2"></el-option>
          <el-option label="中等" value="3"></el-option>
          <el-option label="偏难" value="4"></el-option>
          <el-option label="困难" value="5"></el-option>
        </el-select>
      </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取消</el-button>
        <el-button type="primary" @click=" submitData() ">确定</el-button>
      </div>
    </el-dialog>

    <el-dialog :visible.sync="dialogFormVisible1">
      <el-form
        :model="questionForm"
        label-position="left"
        label-width="90px"
        style="width: 400px; margin-left:50px;"
        status-icon :rules="rules"
      >
        <el-form-item label="机构" prop="institution">
          <el-input  v-model="questionForm.institution" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item label="分类" prop="category">
    <el-select v-model="questionForm.category" placeholder="请选择分类">
      <el-option label="选择题" value="select"></el-option>
      <el-option label="填空题" value="fillout"></el-option>
      <el-option label="判断题" value="judge"></el-option>
      <el-option label="简答题" value="answer"></el-option>
    </el-select>
  </el-form-item>
        <el-form-item label="题目"  prop="question">
          <el-input type="textarea" v-model="questionForm.question" />
        </el-form-item>
        <el-form-item label="答案" prop="standardanswer">
          <el-input type="textarea" v-model="questionForm.standardanswer" />
        </el-form-item>
        <el-form-item label="分数" prop="score">
          <el-input type="number"  v-model="questionForm.score" />
        </el-form-item>
        <el-form-item label="难度" prop="level">
        <el-select v-model="questionForm.level" placeholder="请选择难度">
          <el-option label="容易" value="1"></el-option>
          <el-option label="偏易" value="2"></el-option>
          <el-option label="中等" value="3"></el-option>
          <el-option label="偏难" value="4"></el-option>
          <el-option label="困难" value="5"></el-option>
        </el-select>
      </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible1 = false">取消</el-button>
        <el-button type="primary" @click=" submitData() ">确定</el-button>
      </div>
    </el-dialog>

    <!--<el-button round style="margin-left: 1%;"><i class="el-icon-warning"></i>&nbsp;试题查重</el-button>-->
    <el-button type="danger" round style="margin-left: 1%;" @click="deleteQuestion"><i class="el-icon-delete"></i>&nbsp;删除试题</el-button>
    <el-button :loading="downloadLoading" style="margin-bottom:20px;margin-left: 1%;" type="primary" icon="el-icon-document" @click="handleDownload">
      Excel&nbsp;文件导出
    </el-button>

   <!-- <a href="https://panjiachen.github.io/vue-element-admin-site/feature/component/excel.html" target="_blank" style="margin-left:15px;">
      <el-tag type="info">Documentation</el-tag>
    </a> -->
    <el-table
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
          {{ scope.row.questionsId }}
        </template>
      </el-table-column>
      <el-table-column label="题目">
        <template slot-scope="scope">
          {{ scope.row.question }}
        </template>
      </el-table-column>
      <el-table-column label="分类">
        <template slot-scope="scope">
          {{ scope.row.category }}
        </template>
      </el-table-column>
      <el-table-column label="分数" width="115" align="center">
        <template slot-scope="scope">
          {{ scope.row.score }}
        </template>
      </el-table-column>
      <el-table-column label="机构" width="115" align="center">
        <template slot-scope="scope">
          {{ scope.row.institution }}
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
         @click="edit(row)"
       >
         编辑
       </el-button>
     </template>
      </el-table-column>
    </el-table>
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
  name: 'SelectExcel',
  data() {
    var checkScore= (rule, value, callback) => {
      var reg = /^[0-9]+.?[0-9]*$/;
      if (!reg.test(value)) {
         return callback(new Error('分数必须为数字'));
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
    var checkQuestion= (rule, value, callback) => {
      var newvalue = value.replace(/[^\x00-\xff]/g, "**");
      var length = newvalue.length;
      if (!value) {
        return callback(new Error('问题不能为空'));
      }
      else if(length>64){
        return callback(new Error('超过字数限制'));
      }
    };
    var checkStandardanswer= (rule, value, callback) => {
      var newvalue = value.replace(/[^\x00-\xff]/g, "**");
      var length = newvalue.length;
      if (!value) {
        return callback(new Error('答案不能为空'));
      }
      else if(length>64){
        return callback(new Error('超过字数限制'));
      }
    };
    return {

      list: null,
      page: 1,
      listLoading: true,
      multipleSelection: [],
      downloadLoading: false,
      filename: '',
      input: "",

      questionForm: {
        institution: "",
        category:"",
        question: "",
        standardanswer: "",
        score: "",
        level: "",
      },
      dialogFormVisible: false,
      dialogFormVisible1: false,


      rules:{
        institution: [{validator: checkInstitution,trigger:'blur'}],
        question: [{validator: checkQuestion,trigger:'blur'}],
        standardanswer: [{validator: checkStandardanswer,trigger:'blur'}],
        score: [{validator: checkScore,trigger:'blur'}],
      }
    }
  },
  created() {
    this.fetchData();
  },


  methods: {
    search(){
      console.log(this.page);
      this.$axios({
          url:'http://172.19.18.20:8001/questions/info/',
          method:'get',
          params:{'page':1,'input':this.input},
          data:'',
        }).then((response)=>{
          console.log("successful");
          this.dialogFormVisible = false;
          this.list=response.data.data;
          this.total=response.data.total;
          this.listLoading=false;
          this.page=1;
          console.log(this.list);
        }).catch(error=>{
          console.log('response error');
        });
    },
    givepage(){
      console.log(this.page);
      this.$axios({
          url:'http://172.19.18.20:8001/questions/info/',
          method:'get',
          params:{'page':this.page,'input':this.input},
          data:'',
        }).then((response)=>{
          console.log("successful");
          this.dialogFormVisible = false;
          this.list=response.data.data;
          this.total=response.data.total;
          this.listLoading=false;
          console.log(this.list);
        }).catch(error=>{
          console.log('response error');
        });
    },
    

    handleCreate() {
      this.dialogFormVisible = true;
      //console.log(this.dialogFormVisible);
      this.questionForm = {
        institution: "",
        category: "",
        question: "",
        standardanswer: "",
        score: "",
        level: "",
        questionsId:"",
      };

      //this.dialogFormVisible = true;

    },
    submitData(){
      console.log("22222");
      this.multipleSelection[0]=this.questionForm
      console.log(this.multipleSelection)
      this.questionForm.questionsId=997;
      this.$axios({
        url:'http://172.19.18.20:8001/questions/info/',
        method:'post',
        params:{},
        data:{multipleSelection:this.multipleSelection},
      }).then((response)=>{
        this.dialogFormVisible = false;
        this.dialogFormVisible1 = false;
        this.fetchData();
      }).catch(error=>{
        console.log(error)
      })
    },
    deleteQuestion(){
      var deleteSelection = [].concat(this.multipleSelection);
      console.log(deleteSelection);
      var deleteQuestionsId=[];
      var deleteInstitution=[];
      for(var i=0,len=this.multipleSelection.length; i<len; i++){
        deleteQuestionsId[i]=this.multipleSelection[i].questionsId;
        deleteInstitution[i]=this.multipleSelection[i].institution;
      }
      console.log(deleteQuestionsId);
      console.log(deleteInstitution);
      /*for (const key in this.deleteSelection) {
          // 删除id属性
          delete this.deleteSelection[key].category;
          delete this.deleteSelection[key].question;
          delete this.deleteSelection[key].standardanswer;
          delete this.deleteSelection[key].score;
          delete this.deleteSelection[key].level;
   }*/
      /*this.multipleSelection.forEach((item)=>{
      this.deleteSelection.push(item.questionsId);
      this.deleteSelection.push(item.institution);
　});*/
      const h = this.$createElement;
      console.log(this.multipleSelection);
      //for (var i=0,len=this.multipleSelection.length; i<len; i++){
        //console.log(this.multipleSelection[i].institution);
        //console.log(this.multipleSelection[i].questionsId);
      this.$axios({
        url:'http://172.19.18.20:8001/questions/info/',
        method:'delete',
        data:{},
        params:{'deleteQuestionsId':deleteQuestionsId,'deleteInstitution':deleteInstitution},
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
      this.questionForm=row;
      this.dialogFormVisible1 = true;
      //row.edit=!row.edit;
    },
    fetchData(){
      this.$axios({
        url:'http://172.19.18.20:8001/questions/info/',
        method:'get',
        params:{'page':1,'input':this.input},
        data:{},
      }).then((response)=>{
        console.log("successful");
        this.dialogFormVisible = false;
        this.list=response.data.data;
        this.total=response.data.total;
        this.listLoading=false;
        console.log(this.list);
      }).catch(error=>{
        console.log(error)
      })
    },

    handleSelectionChange(val) {
      this.multipleSelection = val
    },
    handleDownload() {
      if (this.multipleSelection.length) {
        this.downloadLoading = true
        import('@/vendor/Export2Excel').then(excel => {
          const tHeader = ['序号', '机构', '分类', '问题', '答案','分数','难度']
          const filterVal = ['questionsId', 'institution', 'category', 'question', 'standardanswer','score','level']
          const list = this.multipleSelection
          const data = this.formatJson(filterVal, list)
          excel.export_json_to_excel({
            header: tHeader,
            data,
            filename: this.filename
          })
          this.$refs.multipleTable.clearSelection()
          this.downloadLoading = false
        })
      } else {
        this.$message({
          message: 'Please select at least one item',
          type: 'warning'
        })
      }
    },
    formatJson(filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => v[j]))
    }
  }
}
</script>
