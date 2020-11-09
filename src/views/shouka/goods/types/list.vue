<template>
  <div>
    <div class="button-group">
       <el-button type="primary" icon="el-icon-plus" @click="toLink">添加分类</el-button>
    </div>
    <el-table
      stripe
      :data="list"
      header-row-class-name="faka-table-header"
      style="width: 100%"
      v-loading="loading"
    >
      <el-table-column prop="systemNoticeTitle" label="分类名称"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="排序"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="状态"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="创建时间"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="{row}">
          <el-button type="text" @click="toLink(1)">链接</el-button>
          <el-button type="text" @click="toGoods(1)">商品</el-button>
          <el-button type="text" @click="toCard(1)">库存卡</el-button>
          <el-button type="text" @click="toUpdate(1)">编辑</el-button>
          <el-button type="text" @click="toDelete(1)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog
      title="添加分类"
      :visible.sync="addDialog"
      width="30%">
      <add />
    </el-dialog>
    
    <el-dialog
      title="修改分类"
      :visible.sync="updateDialog"
      width="30%">
      <update />
    </el-dialog>
    
    <el-dialog
      title="链接"
      :visible.sync="linkDialog"
      width="50%">
      <link-com />
    </el-dialog>
    
  </div>
</template>

<script>
import add from './add'
import update from './update'
import linkCom from './link'
export default {
  data() {
    return {
      list: [],
      loading: false,
      addDialog: false,
      updateDialog: false,
      linkDialog: false,
      searchForm: {},
    };
  },
  components: {
    add,
    update,
    linkCom
  },
  methods: {
    search() {

    },
    toAdd() {
      this.addDialog = true
    },
    toLink(typeId) {
      this.linkDialog = true
    },
    toGoods(typeId) {
      this.$router.push({ path: '/goodsManager/goods', query: { typeId }})
    },
    toCard(typeId) {
      this.$router.push({ path: '/cardManager/card', query: { typeId }})
    },
    toUpdate(typeId) {
      this.addDialog = true
    },
    toDelete(typeId) {
    },
  },
};
</script>

<style scoped>
.button-group {
  margin-bottom: 15px;
}
</style>