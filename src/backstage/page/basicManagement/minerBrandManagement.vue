
<template>
    <div class="containers">
        <el-row>
            <el-alert
              title="矿机品牌管理"
              :closable="false"
            type="success">
            </el-alert>           
        </el-row>
     
        <el-row class="mt-4 d-flex flex-row flex-nowrap justify-content-end">
            <div style="flex-grow:1" >
                <el-button  icon="el-icon-plus" @click="handleAdd" type="primary">新增</el-button>
            </div>             
            <el-form :inline="true" :model="formInline" class="demo-form-inline">
             <el-form-item>
                  <el-button type="success" @click="reset">重置</el-button>
              </el-form-item>               
              <el-form-item >
                <el-date-picker
                style="width:400px"
                      v-model="formInline.time"
                      type="daterange"
                      value-format="yyyy-MM-dd"
                      range-separator="至"
                      start-placeholder="开始日期"
                      end-placeholder="结束日期">
                </el-date-picker> 
              </el-form-item>              
              <el-form-item>
                <el-button @click="handleSearch" class="mr-4"  icon="el-icon-search"  type="success" circle></el-button>
              </el-form-item>
            </el-form>            
        </el-row>
        <el-table
            :data="tableData"  
            border  
            ref="multipleTable" 
            tooltip-effect="dark"
            highlight-current-row 
            style="width: 100%;font-weight:bold"
            class="mt-3"
            id="darkCell"
            v-loading="loading">
 
            <el-table-column prop="id" label="ID" align="center" width="50"></el-table-column>
            <el-table-column prop="brandName" label="品牌名" align="center" min-width="140"></el-table-column>
            <el-table-column prop="createTime" label="创建时间" align="center" min-width="160">
              <template slot-scope="scope">
                {{scope.row.createTime|dateServer}}
              </template>
            </el-table-column>
            <el-table-column prop="userName" label="创建人" align="center" min-width="120"></el-table-column>
            <el-table-column label="操作" align="center" min-width="150" >
              <template slot-scope="scope">
                <el-button
                  size="mini"
                  @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                <el-button
                  size="mini"
                  type="danger"
                  @click="handleDelete(scope.$index, scope.row)">禁用</el-button>
              </template>
            </el-table-column>            
        </el-table>
        <el-row class="mt-4" v-if="total>0">
             <div style="float:right">
                 <el-pagination
                   @current-change="handleCurrentChange"
                    @size-change="handleSizeChange"
                   :current-page="npage"
                    :page-sizes="[10, 20, 50, 100, 1000,990000]"
                     :page-size="pagesize"
                   background
                   layout="total,sizes,prev, pager, next,jumper"
                   :total="total">
                 </el-pagination>   
             </div>
        </el-row>         
        <el-dialog 
            title="编辑矿机品牌" 
            :visible.sync="dialogFormEditVisible" 
            center
            width="30%"
            >
          <el-form :model="editForm"  ref="editForm" :rules="rules">
            <el-form-item label="矿机品牌" label-width="100px">
              <el-input v-model="editForm.brandName" autocomplete="off"></el-input>
            </el-form-item>            
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormEditVisible = false">取 消</el-button>
            <el-button type="primary" @click="submitForm('editForm')">确 定</el-button>
          </div>
        </el-dialog> 
        <el-dialog 
            title="新增矿机品牌" 
            :visible.sync="dialogFormAddVisible" 
            center
            width="30%"
            >
          <el-form :model="addForm" ref="addForm" :rules="rules" >
            <el-form-item label="矿机品牌" label-width="100px">
              <el-input v-model="addForm.brandName" autocomplete="off"></el-input>
            </el-form-item>
          </el-form>
          <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormAddVisible = false">取 消</el-button>
            <el-button type="primary" @click="submitForm('addForm')">确 定</el-button>
          </div>
        </el-dialog>                
    </div> 
</template>
<script>
import {
  httpSysMinerlist,
  httpSysAddminer,
  httpSysAddminerbrand,
  httpSysGetminerbrandlist,
  httpSysUpdateminerbrand
} from "@/service/http";
import { mapGetters } from "vuex";
import { timeFormat } from "@/config/time";
export default {
  data() {
    return {
      search: {},
      loading: false,
      num: true,
      tableData: [{ MACHINEUID: 1 }],
      npage: 1,
      pagesize: 10,
      total: 0,
      dialogVisible: false,
      formInline: {},
      editForm: {},
      addForm: {},
      dialogFormEditVisible: false,
      dialogFormAddVisible: false,
      rules: {}
    };
  },
  computed: {
    // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      "loginId"
      // ...
    ])
  },
  methods: {
    changeDialog() {},
    handleEdit(index, row) {
      this.editForm = JSON.parse(JSON.stringify(row));
      this.dialogFormEditVisible = true;
    },
    handleAdd() {
      this.dialogFormAddVisible = true;
    },
    handleDelete(index, row) {},
    onSubmit() {},
    /*     重置选择 */
    reset() {
      this.formInline = {};
      this.handleSearch();
    },
    /*   选取第几页 */
    handleCurrentChange(val) {
      this.npage = val;
      this.handleSearch(false);
    },
    /* 选取页数 */
    handleSizeChange(val) {
      this.pagesize = val;
      this.handleSearch(false);
    },
    /* 按条件搜索 */
    handleSearch(type = true) {
      if (type) {
        this.npage = 1;
        this.pagesize = 10;
      }
      if (this.formInline.time && this.formInline.time.length) {
        this.gethttpSysGetminerbrandlist(
          this.npage,
          this.pagesize,
          this.formInline.time[0],
          this.formInline.time[1]
        );
      } else {
        this.gethttpSysGetminerbrandlist(this.npage, this.pagesize);
      }
    },
    /* 提交 */
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          if (formName == "addForm") {
            this.handleAddForm(this.addForm.brandName, this.loginId);
          }
          if (formName == "editForm") {
            this.handleEditForm(
              this.editForm.brandName,
              this.loginId,
              this.editForm.id
            );
          }
        } else {
          return false;
        }
      });
    },
    /*  重置表单 */
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    /*   得到矿机列表 后台管理模块 / 矿机列表展示 */
    gethttpSysGetminerbrandlist(
      npage,
      pagesize,
      begainTimeString,
      endTimeString
    ) {
      this.loading = true;
      httpSysGetminerbrandlist(
        npage,
        pagesize,
        begainTimeString,
        endTimeString
      ).then(res => {
        let data = res.data;
        if (data.code == 200) {
          this.tableData = data.data.list;
          this.total = data.data.allPageNumber;
          this.$message({
            message: "查询成功",
            type: "success"
          });
        } else {
          this.$message({
            message: data.msg,
            type: "error"
          });
        }
        this.loading = false;
      });
    },
    /* 新增矿机品牌 后台管理模块 / 矿机品牌添加 */
    handleAddForm(brandName, loginId) {
      httpSysAddminerbrand(brandName, loginId).then(res => {
        let data = res.data;
        if (data.code == 200) {
          this.$message({
            message: data.msg,
            type: "success"
          });
          this.handleSearch(false);
          this.dialogFormAddVisible = false;
        } else {
          this.$message({
            message: data.msg,
            type: "error"
          });
        }
      });
    },
    /* 新增矿机品牌 后台管理模块 / 矿机品牌修改 */
    handleEditForm(brandName, loginId, id) {
      httpSysUpdateminerbrand(brandName, loginId, id).then(res => {
        let data = res.data;
        if (data.code == 200) {
          this.$message({
            message: data.msg,
            type: "success"
          });
          this.dialogFormEditVisible = false;
          this.handleSearch(false);
        } else {
          this.$message({
            message: data.msg,
            type: "error"
          });
        }
      });
    },

  },
  mounted() {
    this.gethttpSysGetminerbrandlist(this.npage, this.pagesize);
  }
};
</script>
<style lang="less" scoped>
</style>
<style>
#darkCell .cell {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}
</style>


