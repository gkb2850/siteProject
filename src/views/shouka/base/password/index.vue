<template>
  <div>
    <el-card shadow="never">
      <div class="form-div">
        <el-form ref="form" :model="form" label-width="120px">
          <el-form-item label="旧密码">
            <el-input v-model="form.phone" type="password"></el-input>
          </el-form-item>
          <el-form-item label="新密码">
            <el-input v-model="form.phone" type="password"></el-input>
          </el-form-item>
          <el-form-item label="确认密码">
            <el-input v-model="form.phone" type="password"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary">保存设置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: [],
    };
  },
  methods: {
    search() {},
  },
};
</script>

<style scoped>
.form-div {
  width: 600px;
}
</style>