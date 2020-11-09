<template>
  <div>
    <div class="search-container">
      <el-form :inline="true" :model="searchForm" >
        <el-form-item>
          <el-select v-model="searchForm.readState" clearable placeholder="分类">
            <el-option label="卡盟加款机器人" :value="2"></el-option>
          </el-select>
          <el-select v-model="searchForm.readState" clearable placeholder="商品类型">
            <el-option label="掘地求生" :value="1"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" icon="el-icon-search" @click="search">查询</el-button>
        </el-form-item>
      </el-form>
    </div>
    <div class="button-group">
       <el-button type="primary" icon="el-icon-refresh">批量恢复</el-button>
       <el-button type="warning" icon="el-icon-close">批量删除</el-button>
       <el-button type="danger" icon="el-icon-delete">清空回收站</el-button>
    </div>
    <el-table
      stripe
      :data="list"
      header-row-class-name="faka-table-header"
      style="width: 100%"
      v-loading="loading"
    >
      <el-table-column prop="systemNoticeTitle" label="商品分类"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="商品名称"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="商品价格"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="卡号"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="卡密"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="状态"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="创建时间"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="{row}">
          <el-button type="text">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [],
      loading: false,
      searchForm: {},
    };
  },
  methods: {
    search() {

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