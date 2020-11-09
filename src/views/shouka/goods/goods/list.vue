<template>
  <div>
    <div class="search-container">
      <el-form :inline="true" :model="searchForm" >
        <el-form-item>
          <el-select v-model="searchForm.readState" clearable placeholder="分类">
            <el-option label="卡盟加款机器人" :value="2"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-input v-model="searchForm.name" placeholder="商品名称"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" icon="el-icon-search" @click="search">查询</el-button>
        </el-form-item>
        <el-form-item label="对接码">
          <el-input value="114545"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" icon="el-icon-search" @click="toCopy($event)">复制</el-button>
        </el-form-item>
      </el-form>
    </div>
    <div class="button-group">
       <el-button type="primary" icon="el-icon-plus" @click="toAdd">添加商品</el-button>
       <el-button type="primary" icon="el-icon-plus" @click="toExport">添加商品</el-button>
       <el-button type="danger" icon="el-icon-close">批量删除</el-button>
    </div>
    <el-table
      stripe
      :data="list"
      header-row-class-name="faka-table-header"
      style="width: 100%"
      v-loading="loading"
    >
      <el-table-column prop="systemNoticeTitle" label="商品分类"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="排序"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="商品名称"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="价格"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="代理价格"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="库存"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="卖出"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="上下架"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="创建时间"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="{row}">
          <el-button type="text">链接</el-button>
          <el-button type="text">加卡</el-button>
          <el-button type="text">编辑</el-button>
          <el-button type="text">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog
      title="商品链接：掘地求生"
      :visible.sync="linkDialog"
      width="40%">
      <link-com />
    </el-dialog>
      <!-- 导出 -->
    <el-dialog
      title="导出"
      :visible.sync="exportDialog"
      width="40%">
      
      <daochu />
    </el-dialog>
    
  </div>
</template>

<script>
import clip from "@/utils/clipboard";
import daochu from '../daochu'
import linkCom from './link'
export default {
  data() {
    return {
      list: [],
      loading: false,
      linkDialog: false,
      exportDialog: false,
      searchForm: {},
    };
  },
  components: {
    daochu
  },
  methods: {
    search() {

    },
    toAdd() {
      this.$router.push('/goodsManager/add')
    },
    toLink(goodId) {
      this.linkDialog = true
    },
    toCard(goodId) {
      this.$router.push('/cardManager/cardAdd')
    },
    toUpdate(goodId) {
      this.$router.push('/goodsManager/update')
    },
    toDelete(goodId) {
    },
    toExport() {
      this.exportDialog = true
    }
  },
};
</script>

<style scoped>
.search-container .el-form {
  padding: 0;
}
.button-group {
  margin-bottom: 15px;
}
</style>