<template>
  <div>
    <div class="info-content">只保留近30天的登录日志</div>
    <el-table
      stripe
      :data="logList"
      header-row-class-name="faka-table-header"
      style="width: 100%"
      v-loading="loading"
    >
      <el-table-column prop="systemNoticeTitle" label="商户ID"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="商户账号"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="登录时间"></el-table-column>
      <el-table-column prop="systemNoticeTitle" label="登录IP"></el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      logList: []
    };
  },
};
</script>

<style scoped>
.info-content {
  background-color: #d6f6fb;
  height: 50px;
  color: #35b8e0;
  border: 1px solid #a5ecf7;
  font-size: 14px;
  display: flex;
  align-items: center;
  padding-left: 20px;
  margin-bottom: 15px;
}
</style>