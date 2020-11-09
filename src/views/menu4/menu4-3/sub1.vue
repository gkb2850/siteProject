<template>
  <div class="container menu2-1-container">
    <div class="filter-wrap">
      <el-button type="primary" @click="getOneLevel">对接一级目录</el-button>
    </div>

   
    <el-table row-key="catalogID" ref="multipleTable" :data="listData" style="width: 100%" @selection-change="handleSelectionChange" :header-cell-style="{background:'#eef1f6',color:'#606266'}" v-loading="loading" :tree-props="{children: 'children'}">
      <!-- <el-table-column :show-overflow-tooltip="true" type="selection" > </el-table-column>-->
       <el-table-column :show-overflow-tooltip="true" prop="catalogName" label="目录名称"></el-table-column>
       <el-table-column :show-overflow-tooltip="true" prop="catalogID" label="目录编号" align="center"></el-table-column>
       <el-table-column :show-overflow-tooltip="true" prop="sort" label="序号" align="center"> </el-table-column>
       <el-table-column :show-overflow-tooltip="true" prop="test6" label="对接状态" align="center">
        <template slot-scope="scope">
          <el-button type="danger" plain size="mini" style="margin-left:0" v-show="scope.row.agentType == 0">禁止对接</el-button>
          <el-button type="warning" plain size="mini" style="margin-left:0" v-show="scope.row.agentType == 1">单次对接</el-button>
          <el-button type="primary" plain size="mini" style="margin-left:0" v-show="scope.row.agentType == 2">多次对接</el-button>
        </template>
      </el-table-column>
       <el-table-column :show-overflow-tooltip="true" prop="test8" label="操作" align="center">
        <template slot-scope="scope">
          <el-button type="text" size="mini" v-show="!scope.row.parentID" @click="secondLevel(scope.row)" icon="el-icon-share">对接子级目录</el-button>
          <el-button type="text" size="mini" v-show="scope.row.parentID" icon="el-icon-share" @click="goGoodsList(scope.row)">对接目录商品</el-button>
        </template>
      </el-table-column>
    </el-table>

    

    <!-- 一级目录对接-->
    <el-dialog title="对接一级目录" :visible.sync="dialog1" class="dialog1" :close-on-click-modal="false">
      <el-form ref="form" :model="form" label-width="100px">
        <el-form-item label="选择目录：">
          <el-checkbox :indeterminate="isIndeterminate" v-model="checkAll" @change="handleCheckAllChange">全选</el-checkbox>
          <div style="margin: 15px 0;"></div>
          <el-checkbox-group v-model="checkedCities" @change="handleCheckedCitiesChange">
            <el-checkbox v-for="oneLevel in listData" :label="oneLevel.catalogID" :key="oneLevel.catalogID">{{oneLevel.catalogName}}</el-checkbox>
          </el-checkbox-group>
        </el-form-item>
        
      </el-form>
      
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialog1 = false">取 消</el-button>
        <el-button type="primary" @click="goOneLevel" :loading="confirmLoading">确 定</el-button>
      </span>
    </el-dialog>


    <!-- 二级目录对接-->
    <el-dialog title="对接二级目录" :visible.sync="dialog2" class="dialog1" :close-on-click-modal="false">
      <el-form :model="ruleForm" ref="ruleForm" label-width="160px">
        <el-form-item label="本地目录">
          <el-select v-model="ruleForm.catalogID" placeholder="请选择本地目录">
            <el-option
              v-for="item in localCatalogData"
              :key="item.catalogID"
              :label="item.catalogName"
              :value="item.catalogID">
            </el-option>
          </el-select>
        </el-form-item>

        <el-form-item label="选择对接目录：">
          <el-checkbox-group v-model="checkedCities" @change="handleCheckedCitiesChange">
            <el-checkbox v-for="data in secondLevelData" :label="data.catalogID" :key="data.catalogID" :disabled = "data.isAgent===1">{{data.catalogName}}</el-checkbox>
          </el-checkbox-group>
        </el-form-item>
        
      </el-form>
      
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialog2 = false">取 消</el-button>
        <el-button type="primary" @click="goSecondLevel" :loading="confirmLoading">确 定</el-button>
      </span>
    </el-dialog>


    <!-- 对接商品 -->
    <el-dialog title="对接商品" :visible.sync="dialog3" class="dialog1" :close-on-click-modal="false">
      <el-form :model="ruleForm" ref="ruleForm" label-width="160px">
        <el-form-item label="本地目录">
          <!-- <el-select v-model="ruleForm.catalogID" placeholder="请选择本地目录">
            <el-option
              v-for="item in localCatalogData"
              :key="item.catalogID"
              :label="item.catalogName"
              :value="item.catalogID">
            </el-option>
          </el-select> -->
          <el-cascader v-model="ruleForm.catalogID" :options="recombinationData" :props="optionProps">
            <template slot-scope="{ data }">
              <span>{{ data.catalogName }}</span>
            </template>
          </el-cascader>
        </el-form-item>
      </el-form>
      
    <el-table border row-key="goodsID" ref="goodsListTable" :data="goodsList" style="width: 100%" @selection-change="handleSelectionChangeGoods" :header-cell-style="{background:'#f5f5f5',color:'#606266'}">
         <el-table-column :show-overflow-tooltip="true" type="selection" :selectable='isDisabled'> </el-table-column>
         <el-table-column :show-overflow-tooltip="true" prop="goodsCode" label="编号"  ></el-table-column>
         <el-table-column :show-overflow-tooltip="true" prop="goodsName" label="商品名称" min-width="400px"></el-table-column>
       <el-table-column :show-overflow-tooltip="true" prop="test6" label="对接状态" align="center">
        <template slot-scope="scope">
          <el-button type="danger" plain size="mini" style="margin-left:0" v-show="scope.row.agentType == 0">禁止对接</el-button>
          <el-button type="warning" plain size="mini" style="margin-left:0" v-show="scope.row.agentType == 1">单次对接</el-button>
          <el-button type="primary" plain size="mini" style="margin-left:0" v-show="scope.row.agentType == 2">多次对接</el-button>
        </template>
      </el-table-column>

      </el-table>


      <span slot="footer" class="dialog-footer">
        <el-button @click="dialog3 = false">取 消</el-button>
        <el-button type="primary" @click="goGoods" :loading="confirmLoading">确 定</el-button>
      </span>
    </el-dialog>


  </div>
</template>

<script>
import Tinymce from '@/components/Tinymce'
import { getLists,getDatas } from '@/api/price/price-1/common'
export default {
  data() {
    return {
      optionProps:{
        value: 'catalogID',
        label: 'catalogName',
      },
      type:1,
      color:'#333',
      form:{},
      ruleForm: {},
      dialog1:false, // 是否展示一级目录
      dialog2:false, // 是否展示二级目录
      dialog3:false, // 是否对接商品
      currentPage:1,
      tableData: [],
      queryParams:{},
      checkList:[],
      listData:[],
      loading:true,
      multipleSelection: [],
      goodsUpData: [],  // 对接商品选择的商品ID
      isIndeterminate: false,// 操作一级目录
      checkAll: false,// 操作一级目录
      checkedCities: [],// 操作一级目录
      confirmLoading: false,
      localCatalogData: [], // 本地目录
      secondLevelData: [],
      recombinationData: [],// 重组数据--本地目录
      goodsList: [] // 可对接商品
    }
  },
  components: {
    Tinymce
  },
  mounted() {
    if (this.$route.params && this.$route.params.id) {
      this.queryParams = {
        siteID:this.$route.params.id
      }
      this.getList()
      this.localCatalog()
      this.getCatalogTree()
    }
  },
  methods: {
    // 获取目录列表
    getList() {
      this.loading = true
      getDatas(this.queryParams,'/goods/catalog/agentTree').then(res => {
        if (res.code===1001){
          // res.body.map(item => {
          //     item.hasChildren = true
          // })
          this.listData = res.body
          this.loading = false
        }
      }).catch(err => {

      })
    },

    // 获取本地目录
    localCatalog() {
      getDatas({},'/goods/catalog/catalogList').then(res => {
        if (res.code===1001){
          this.localCatalogData = res.body
        }
      }).catch(err => {

      })
    },

    // 获取一级目录
    getOneLevel(){
      this.dialog1 = true
      this.confirmLoading = false
      this.checkAll = false
      this.checkedCities =[]
    },

    handleCheckAllChange(val) {
      this.checkedCities = [];
      if(val){
          let catalogID = [];
          this.listData.map(item => {
              catalogID.push(item.catalogID)
          })
        this.checkedCities = catalogID
      }
      this.isIndeterminate = false;
    },
    handleCheckedCitiesChange(value) {
      let checkedCount = value.length;
      this.checkAll = checkedCount === this.listData.length;
      this.isIndeterminate = checkedCount > 0 && checkedCount < this.listData.length;
    },

    // 提交一级目录
    goOneLevel(){
      this.confirmLoading = true
      if(this.checkedCities.length===0){
        this.$message({
          showClose: true,
          message: '请选择要对接的一级目录',
          type: 'error',
        });
      }else{
        getLists({sourceCatalogID:this.checkedCities},'/goods/catalog/batchCatalog').then(res => {
          if (res.code===1001){
            this.$message({
              message: res.msg,
              type: 'success',
            });
            this.getList()
            this.confirmLoading = false
            this.dialog1 = false
            this.checkAll = false
            this.checkedCities =[]
          }else{
            this.$message({
              showClose: true,
              message: res.msg,
              type: 'error',
            });
            this.confirmLoading = false
          }
        }).catch(err => {
            this.confirmLoading = false
        })
      }
    },



    //  对接二级目录

    secondLevel(data){
      this.checkedCities =[]
      this.ruleForm = {}
      if(data.children.length===0){
        this.$message({
          showClose: true,
          message: '当前目录没有子级目录哟~',
          type: 'error',
        });
      }else{
          this.secondLevelData = data.children
          this.dialog2 = true
      }
    },

    // 对接二级目录
    goSecondLevel(){
      if(!this.ruleForm.catalogID){
        this.$message({
          showClose: true,
          message: '请选择本地目录',
          type: 'error',
        });
      }else if(this.checkedCities.length===0){
        this.$message({
          showClose: true,
          message: '请选择对接目录',
          type: 'error',
        });
      }else{
        this.confirmLoading = true
        getLists({sourceCatalogID:this.checkedCities,catalogID:this.ruleForm.catalogID},'/goods/catalog/batchCatalog').then(res => {
          if (res.code===1001){
            this.$message({
              message: res.msg,
              type: 'success',
            });
            this.getList()
            this.confirmLoading = false
            this.dialog2 = false
            this.checkedCities =[]
          }else{
            this.$message({
              showClose: true,
              message: res.msg,
              type: 'error',
            });
            this.confirmLoading = false
          }
        }).catch(err => {
            this.confirmLoading = false
        })
      }
    },


    // 获取可对接的商品
    goGoodsList(data){
    //let recombinationData = []
    //if(this.localCatalogData.length>0){
    //    this.listData.map(item => {
    //        if(item.children.length>0){
    //            let newData = []
    //            item.children.map(items => {
    //                newData.push({catalogID:items.catalogID,catalogName:items.catalogName})
    //            })
    //            recombinationData.push({catalogID:item.catalogID,catalogName:item.catalogName,disabled:false,children:newData})
    //        }else{
    //            recombinationData.push({catalogID:item.catalogID,catalogName:item.catalogName,disabled:true})
    //        }
    //    })
    //}
    //this.recombinationData = recombinationData
      this.ruleForm = {}
      getDatas({catalogID:data.catalogID},'/goods/goods/goodsListByCatalog').then(res => {
        if (res.code===1001){
          this.goodsList = res.body
          this.dialog3 = true
        }
      }).catch(err => {

      })
    },
    //  对接商品
    goGoods() {
      let goodsID = []
      this.goodsUpData.map(item => {
          goodsID.push(item.goodsID)
      })
      if(!this.ruleForm.catalogID){
        this.$message({
          showClose: true,
          message: '请选择本地目录',
          type: 'error',
        });
      }else if(goodsID.length===0){
        this.$message({
          showClose: true,
          message: '请选择对接商品',
          type: 'error',
        });
      }else{
        this.confirmLoading = true
        getLists({sourceGoodsIDs:goodsID,catalogID:this.ruleForm.catalogID[1]},'/goods/goods/batchGoods').then(res => {
          if (res.code===1001){
            this.$message({
              message: res.msg,
              type: 'success',
            });
            this.getList()
            this.confirmLoading = false
            this.dialog3 = false
          }else{
            this.$message({
              showClose: true,
              message: res.msg,
              type: 'error',
            });
            this.confirmLoading = false
          }
        }).catch(err => {
            this.confirmLoading = false
        })
      }
    },

    // 获取本地目录tree
    getCatalogTree(){
      getDatas({},'/goods/catalog/tree').then(res => {
        if (res.code===1001){
          let recombinationData = []
          res.body.map(item => {
              if(item.children.length>0){
                  let newData = []
                  item.children.map(items => {
                      newData.push({catalogID:items.catalogID,catalogName:items.catalogName})
                  })
                  recombinationData.push({catalogID:item.catalogID,catalogName:item.catalogName,disabled:false,children:newData})
              }else{
                  recombinationData.push({catalogID:item.catalogID,catalogName:item.catalogName,disabled:true})
              }
          })
          
          this.recombinationData = recombinationData
          //this.catalogTreeList = res.body
        }
      }).catch(err => {

      })

    },

    // 部分禁用
    isDisabled(row,index){
        if(row.isAgent == 0 || !row.isAgent){
          return true;
        }else{
          return false;
        }
    },



    handleSelectionChange(val) {
      this.multipleSelection = val;
    },

    handleSelectionChangeGoods(val) {
      this.goodsUpData = val
    }

  }
}
</script>
<style lang="scss">
.menu2-1-container {
  .dialog1 {
    .el-table__header-wrapper {
      //display:none;
    }
    .el-dialog__body {
      overflow: auto;
      max-height:700px;
    }
    .el-input {
      // width:400px;
    }
  }
}
</style>
<style scoped lang="scss">
.container {
  padding:20px;
  .tips {
    color:#909399;
    font-size:12px;
    line-height:20px;
    width:400px;
    margin-left:10px;
  }
  .tab-wrap {
    a {
      &.active {
        border-bottom:2px solid #409eff
      }
    }
  }
  .filter-wrap {
    margin-top:20px;
  }
  .handle-wrap {
    // margin-top:20px;
    margin-bottom:20px;
  }
  
}
</style>

