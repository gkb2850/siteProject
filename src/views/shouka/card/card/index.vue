<template>
  <div>
    <div class="search-container">
      <el-form :inline="true" :model="searchForm">
        <el-form-item>
          <el-select v-model="searchForm.readState" clearable placeholder="分类">
            <el-option label="卡盟加款机器人" :value="2"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-select v-model="searchForm.readState" clearable placeholder="商品类型">
            <el-option label="掘地求生" :value="1"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-select v-model="searchForm.status" clearable placeholder="状态">
            <el-option label="已售出" :value="1"></el-option>
            <el-option label="未售出" :value="2"></el-option>
          </el-select>
        </el-form-item>
        <span v-show="searchForm.status === 1">
          <el-form-item>
            <el-input v-model="searchForm.orderId" clearable placeholder="请输入订单号" />
          </el-form-item>
          <el-form-item>
            <el-input v-model="searchForm.orderId" clearable placeholder="请输入联系方式" />
          </el-form-item>
        </span>
        <el-form-item>
          <el-button type="primary" icon="el-icon-search" @click="search">查询</el-button>
        </el-form-item>
      </el-form>
    </div>
    <div class="button-group">
      <el-button type="primary" icon="el-icon-plus" @click="toAdd">添加虚拟卡</el-button>
      <el-button type="success" icon="el-icon-upload2" @click="toExport">导出卡密</el-button>
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
      <el-table-column prop="systemNoticeTitle" label="商品名称"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="商品价格"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="卡号"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="卡密"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="状态"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="创建时间"></el-table-column>
      <div v-if="searchForm.status === 1">
        <el-table-column prop="systemNoticeTitle" label="售出时间"></el-table-column>
        <el-table-column prop="systemNoticeTitle" label="取卡密码"></el-table-column>
        <el-table-column prop="systemNoticeTitle" label="联系方式"></el-table-column>
      </div>
      <el-table-column label="操作">
        <template slot-scope="{row}">
          <el-button type="text">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 导出 -->
    <el-dialog title="导出" :visible.sync="exportDialog" width="40%">
      <daochu />
    </el-dialog>

    <!-- 导出 -->
  </div>
</template>

<script>
import daochu from "../daochu";
export default {
  data() {
    return {
      list: [],
      loading: false,
      exportDialog: false,
      searchForm: {},
    };
  },
  components: {
    daochu,
  },
  methods: {
    search() {},
    toAdd() {
      this.$router.push("/cardManager/cardAdd");
    },
    toExport() {
      this.exportDialog = true;
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