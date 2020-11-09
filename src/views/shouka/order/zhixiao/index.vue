<template>
  <div>
    <div class="search-container">
      <el-form :inline="true" :model="searchForm">
        <el-form-item>
          <el-input v-model="searchForm.orderId" clearable placeholder="请输入订单号" />
        </el-form-item>
        <el-form-item>
          <el-input v-model="searchForm.orderId" clearable placeholder="请输入商品名称" />
        </el-form-item>
        <el-form-item>
          <el-select v-model="searchForm.readState" clearable placeholder="商品类型">
            <el-option label="卡密" :value="2"></el-option>
            <el-option label="充值" :value="2"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-input v-model="searchForm.orderId" clearable placeholder="请输入卡号" />
        </el-form-item>
        <el-form-item>
          <el-date-picker
            v-model="searchForm.time"
            type="daterange"
            range-separator="至"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
          ></el-date-picker>
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
      <el-table-column prop="systemNoticeTitle" label="订单号"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="商品名称"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="商品类型"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="供应商"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="购买客户"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="购价"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="数量"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="销售日期"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="订单状态"></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="{row}">
          <el-button type="text">订单详情</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog title="订单详情记录" :visible.sync="infoDialog" width="50%">
      <info />
    </el-dialog>

  </div>
</template>

<script>
import info from '../info'
export default {
  data() {
    return {
      list: [],
      loading: false,
      infoDialog: false,
      searchForm: {},
    };
  },
  components: {
    info
  },
  methods: {
    search() {},
    toInfo() {
      this.infoDialog = true
    },
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