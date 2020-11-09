<template>
  <div class="container container-good">
    <div class="filter-wrap">
      <span>商品名称：</span>
      <el-input placeholder="请输入商品名称" size="mini" v-model="tableDataInfo.goodsName" class="search-input"></el-input>
      <span style="padding-left:20px">用户名称：</span>
      <el-input placeholder="请输入用户名称" size="mini" v-model="tableDataInfo.userName" class="search-input"></el-input>
      <span style="padding-left:20px">密价类型：</span>
      <el-select v-model="tableDataInfo.type" size="mini" placeholder="密价类型">
        <el-option label="用户密价" value="1"></el-option>
        <el-option label="站点密价" value="2"></el-option>
      </el-select>
      <el-button style="margin-left:20px" type="primary" icon="el-icon-search" @click="findList" :loading="buttonLoading">查询</el-button>
      <el-button type="danger" icon="el-icon-edit" @click="addSecretPrice">新增密价</el-button>
    </div>
    <el-table border row-key="id" ref="multipleTable" :data="tableData" v-loading="loading" style="width: 100%" @selection-change="handleSelectionChange" :header-cell-style="{background:'#f5f5f5',color:'#606266'}" >
       <el-table-column :show-overflow-tooltip="true" prop="goodsID" label="编号" align="center" ></el-table-column>
       <el-table-column :show-overflow-tooltip="true" prop="goodsName" label="商品名称"></el-table-column>
       <el-table-column :show-overflow-tooltip="true" prop="goodsPrice" label="商品价格" ></el-table-column>
       <el-table-column :show-overflow-tooltip="true" prop="price" label="商品密价" ></el-table-column>
       <el-table-column :show-overflow-tooltip="true" prop="label7" label="操作" align="center" >
        <template slot-scope="scope">
          <el-button type="text" size="mini" @click="getUserList(scope.row)">设置用户密价</el-button>
          <el-button type="text" size="mini" @click="getSiteList(scope.row)">设置站点密价</el-button>
          <el-button type="text" size="mini" @click="_del(scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination background @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="currentPage" :page-sizes="[10, 20, 30]" :page-size="tableDataInfo.pageSize" layout="total, sizes, prev, pager, next, jumper" :total="tableDataInfo.total"> </el-pagination>

    <!-- 设置用户密价 -->
    <el-dialog title="设置用户密价" :visible.sync="dialog1" width="70%" class="dialog6">
      <div class="title">{{thisData.goodsName}}</div>
      <div class="filter-wrap">
        <span>用户编号：</span>
        <el-input v-model="tableUser.localUserID" placeholder="请输入用户编号" class="search-input"></el-input>
        <span>用户名称：</span>
        <el-input v-model="tableUser.userName" placeholder="请输入用户名称" class="search-input"></el-input>
        <el-button type="primary" icon="el-icon-search" @click="clickFind(1),buttonLoading=true" :loading="buttonLoading">查询</el-button>
      </div>
      <el-table border row-key="id" ref="multipleTable" v-loading="loading" :data="tableData2" style="width: 100%" :header-cell-style="{background:'#f5f5f5',color:'#606266'}" >
         <el-table-column :show-overflow-tooltip="true" prop="localUserID" label="用户编号" align="center"></el-table-column>
         <el-table-column :show-overflow-tooltip="true" prop="userName" label="用户名称" align="center"></el-table-column>
         <el-table-column :show-overflow-tooltip="true" prop="userLevelName" label="级别" align="center"></el-table-column>
         <el-table-column :show-overflow-tooltip="true" prop="label4" label="状态" align="center">
            <template slot-scope="scope">
                <span>{{scope.row.userState === 1 ? '正常' : '禁用'}}</span>
            </template>
        </el-table-column>
         <el-table-column :show-overflow-tooltip="true" prop="money" label="余额" align="center"></el-table-column>
         <el-table-column :show-overflow-tooltip="true" prop="label7" label="操作" align="center" width="400">
          <template slot-scope="scope">
            <el-input-number v-model="scope.row.secretPrice" controls-position="right" size="mini" :min="0" :precision="2"  />
            <el-button type="text" size="mini" @click="secretUserPrice(scope.row)">设置</el-button>
            <!-- <el-button size="mini" disabled="">清空</el-button> -->
          </template>
        </el-table-column>
      </el-table>
      <el-pagination background @size-change="handleSizeChangeUser" @current-change="handleCurrentChangeUser" :current-page="currentPage" :page-sizes="[10, 20, 30]" :page-size="20" layout="total, sizes, prev, pager, next, jumper" :total="tableUser.total"> </el-pagination>
    </el-dialog>

    <!-- 设置站点密价 -->
    <el-dialog title="设置站点密价" :visible.sync="dialog2" width="70%" class="dialog6">
      <div class="title">{{thisData.goodsName}}</div>
      <div class="filter-wrap">
        <span>站点编号：</span>
        <el-input v-model="tableSite.siteID" placeholder="请输入站点编号" class="search-input"></el-input>
        <span>站点名称：</span>
        <el-input v-model="tableSite.siteName" placeholder="请输入站点名称" class="search-input"></el-input>
        <el-button type="primary" icon="el-icon-search" @click="clickFind(2),buttonLoading=true" :loading="buttonLoading">查询</el-button>
      </div>
      <el-table border row-key="id" ref="multipleTable" v-loading="loading" :data="tableData3" style="width: 100%" :header-cell-style="{background:'#f5f5f5',color:'#606266'}" >
         <el-table-column :show-overflow-tooltip="true" prop="siteID" label="站点编号" align="center"></el-table-column>
         <el-table-column :show-overflow-tooltip="true" prop="siteName" label="站点名称" align="center"></el-table-column>
         <el-table-column :show-overflow-tooltip="true" prop="contacts" label="联系人" align="center"></el-table-column>
         <el-table-column :show-overflow-tooltip="true" prop="domainPre" label="站点前缀" align="center"></el-table-column>
         <el-table-column :show-overflow-tooltip="true" prop="label7" label="操作" align="center" width="400">
          <template slot-scope="scope">
            <el-input-number v-model="scope.row.secretPrice" controls-position="right" size="mini" :min="0" :precision="2"  />
            <el-button type="text" size="mini" @click="secretSitePrice(scope.row)">设置</el-button>
            <!-- <el-button size="mini" disabled="">清空</el-button> -->
          </template>
        </el-table-column>
      </el-table>
      <el-pagination background @size-change="handleSizeChangeSite" @current-change="handleCurrentChangeSite" :current-page="currentPage" :page-sizes="[10, 20, 30]" :page-size="20" layout="total, sizes, prev, pager, next, jumper" :total="tableSite.total"> </el-pagination>
    </el-dialog>

    <!-- 删除 -->
    <el-dialog title="确认要删除此密价吗？" :visible.sync="delDialog" width="30%" > 
      <span slot="footer" class="dialog-footer">
        <el-button @click="delDialog = false">取 消</el-button>
        <el-button type="primary" @click="delList" :loading="delConfirmLoading">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import Sortable from 'sortablejs'
import { getList} from '@/api/meun3/menu3-6/index'
import { getLists,getDatas } from '@/api/price/price-1/common'
export default {
  data() {
    return {
      day:true,
      color:'#333',
      step:1,
      form:{},
      dialog1:false,
      dialog2:false,
      buttonLoading: false,
      delDialog: false,
      delConfirmLoading: false,
      delId:'',
      tableData: [],
      tableData2:[],
      tableData3:[],
      multipleSelection: [],
      currentPage: 1,
      loading: false,
      tableDataInfo: {
        pageNum: 1,// 当前页数
        pageSize: 20, //每页条数
        total: 0, //数据总共条数
      },
      tableUser: {
        pageNum: 1,// 当前页数
        pageSize: 20, //每页条数
        total: 0, //数据总共条数
      },
      tableSite: {
        pageNum: 1,// 当前页数
        pageSize: 20, //每页条数
        total: 0, //数据总共条数
        goodsID:'', //用于排除已设置密价的站点
      },
      thisData:[] // 当前商品信息
    }
  },
  mounted() {
    this.getList()
  },
  methods: {
    handleSizeChange(val) {
      this.tableDataInfo = {
        ...this.tableDataInfo,
        pageNum: 1,
        pageSize: val,
      }
      this.getList()
    },
    handleCurrentChange(val) {
      this.tableDataInfo = {
        ...this.tableDataInfo,
        pageNum: val
      }
      this.getList()
    },


    //密价用户列表
    handleSizeChangeUser(val) {
      this.tableUser = {
        ...this.tableUser,
        pageSize: val
      }
      this.getUserList(this.thisData)
    },
    handleCurrentChangeUser(val) {
      this.tableUser = {
        ...this.tableUser,
        pageNum: val
      }
      this.getUserList(this.thisData)
    },

    //密价站长列表
    handleSizeChangeSite(val) {
      this.tableSite = {
        ...this.tableSite,
        pageSize: val
      }
      this.getSiteList(this.thisData)
    },
    handleCurrentChangeSite(val) {
      this.tableSite = {
        ...this.tableSite,
        pageNum: val
      }
      this.getSiteList(this.thisData)
    },




    handleSelectionChange(val) {
      this.multipleSelection = val;
    },

    // 新增密价
    addSecretPrice(){
      this.dialog1= true
    },

    // 查询
    clickFind(val){
      if(val===1){
        this.getUserList(this.thisData)

      }else{
        this.getSiteList(this.thisData)

      }
    },


    // 获取用户并设置密价
    getUserList(data) {
      this.dialog1 = true
      this.thisData =data
      this.loading = true
      getLists(this.tableUser,'/user/user/secretUser').then(res => {
        if (res.code===1001){
          this.tableData2 = res.body.records
          this.tableUser = {
            ...this.tableUser,
            pageNum: res.body.current,
            pageSize: res.body.size,
            total: res.body.total 
          }
        }
        this.loading = false
        this.buttonLoading = false
      }).catch(err => {
        this.loading = false
        this.buttonLoading = false
      })
    },

    // 获取站点并设置密价
    getSiteList(data) {
      this.dialog2 = true
      this.thisData =data
      this.loading = true
      this.tableSite={...this.tableSite,goodsID:data.goodsID}
      getLists(this.tableSite,'/platform/site/secretSite').then(res => {
        if (res.code===1001){
          this.tableData3 = res.body.records
          this.tableSite = {
            ...this.tableSite,
            pageNum: res.body.current,
            pageSize: res.body.size,
            total: res.body.total 
          }
        }
        this.loading = false
        this.buttonLoading = false
      }).catch(err => {
        this.loading = false
        this.buttonLoading = false
      })
    },


    // 设置用户密价
    secretUserPrice(data) {
      if(!data.secretPrice){
        this.$message({
          showClose: true,
          message: '密价不能为空',
          type: 'error'
        });
      }else{
        let payload = {
          goodsID: this.thisData.goodsID,
          localUserID: data.localUserID,
          price: data.secretPrice
        }
        getLists({...payload},'/goods/secretPrice/saveSecretPrice').then(res => {
          if (res.code===1001){
            this.$message({
              showClose: true,
              message: res.msg,
              type: 'success'
            });
            this.getUserList(this.thisData)
            this.getList()
          }else{
            this.$message({
              showClose: true,
              message: res.msg,
              type: 'error'
            });
          }
        }).catch(err => {
          
        })
      }
    },

    // 设置站点密价
    secretSitePrice(data) {
      if(!data.secretPrice){
        this.$message({
          showClose: true,
          message: '密价不能为空',
          type: 'error'
        });
      }else{
        let payload = {
          goodsID: this.thisData.goodsID,
          secretSiteID: data.siteID,
          price: data.secretPrice
        }
        getLists({...payload},'/goods/secretPrice/saveSiteSecretPrice').then(res => {
          if (res.code===1001){
            this.$message({
              showClose: true,
              message: res.msg,
              type: 'success'
            });
            this.getSiteList(this.thisData)
            this.getList()
          }else{
            this.$message({
              showClose: true,
              message: res.msg,
              type: 'error'
            });
          }
        }).catch(err => {
          
        })
      }
    },

    // 查询密价列表
    getList() {
      getList(this.tableDataInfo).then(res => {
        this.tableData = res.body.records
        this.tableDataInfo = {
          ...this.tableDataInfo,
          pageNum: res.body.current,
          pageSize: res.body.size,
          total: res.body.total 
        }
        this.loading = false
        setTimeout(()=>{
          this.buttonLoading = false
        },1500)
      }).catch(err => {
        this.loading = false
        setTimeout(()=>{
          this.buttonLoading = false
        },1500)
      })
    },

    // 点击查询密价列表
    findList(){
      this.tableDataInfo.pageNum = 1
      this.buttonLoading = true
      this.getList()
    },

    _del(data){
        this.delDialog=true
        this.delId=data.secretPriceID
    },

    // 删除密价
    delList() {
      this.delConfirmLoading = true
      getDatas({id:this.delId},'/goods/secretPrice/delete').then(res => {
          if (res.code===1001){
            this.$message({
              showClose: true,
              message: res.msg,
              type: 'success'
            });
            this.getList()
          }else{
            this.$message({
              showClose: true,
              message: res.msg,
              type: 'error'
            });
          }
          this.delDialog=false
          this.delConfirmLoading = false
        }).catch(err => {
          
        })
    }
  }
}
</script>

<style lang="scss" scoped>

  .el-color-picker {
    float:left;
    margin-left:20px;
  }
.container {
  .go-btn {
    border:1px solid #ccc;
    font-weight: bold;
    font-size:13px;
    padding:1px 5px;
    border-radius: 3px;
    display:inline-block;
    color:#409EFF;
    cursor: pointer;
    transition:.3s;
    .go-btn.no {
      color:#909399;
    }
  }

  .go-btn:hover {
    transition:.3s;
    border:1px solid #409EFF;
  }
  .tips {
    color:#909399;
    font-size:12px;
    line-height:20px;
    width:400px;
  }
  padding:20px;
  .filter-wrap {
    .search-input {
      width:200px;
    }
    .handle-wrap {
      margin-top:20px;
    }
  }
  .el-form {
    padding:0;
    .el-input,.el-textarea {
      width:400px;
    }
  }
}
</style>

