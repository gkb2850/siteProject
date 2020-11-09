<template>
  <div>
    <div class="search-container">
      <el-form :inline="true" :model="searchForm">
        <el-form-item>
          <el-input v-model="searchForm.orderId" clearable placeholder="商品池名" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" icon="el-icon-search" @click="search">查询</el-button>
        </el-form-item>
      </el-form>
    </div>
    <el-table
      stripe
      :data="list"
      header-row-class-name="faka-table-header"
      style="width: 100%"
      v-loading="loading"
    >
      <el-table-column prop="systemNoticeTitle">
        <template slot-scope="{row}">
          <el-tag type="danger">年费置顶</el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="systemNoticeTitle" label="内容"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="商家QQ"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="对接码"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="添加时间"></el-table-column>
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
    search() {},
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