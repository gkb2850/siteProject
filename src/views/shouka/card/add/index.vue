<template>
  <div class="dialog">
    <el-tabs v-model="activeName" type="border-card">
      <el-tab-pane label="添加" name="add">
        <el-form class="add-form" ref="form" :model="addForm" label-width="120px">
          <el-form-item label="指定商品">
            <el-select v-model="addForm.readState" style="width: 100%" clearable placeholder="商品类型">
              <el-option label="掘地求生" :value="1"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="导入格式">
            <el-select
              v-model="addForm.importFormat"
              style="width: 100%"
              clearable
              placeholder="商品类型"
            >
              <el-option label="自动识别" :value="1"></el-option>
              <el-option label="用' '分隔" :value="2"></el-option>
              <el-option label="用'|'分隔" :value="3"></el-option>
              <el-option label="用','分隔" :value="4"></el-option>
              <el-option label="用'----'分隔" :value="5"></el-option>
              <el-option label="不分隔" :value="6"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="格式演示">
            AAAAAAAA BBBBBBBB
            <br />卡号+空格+密码，一行一张卡
            <br />如：A1B2C3D4F5E8 9876543210
            <br />最多一次添加500张(500行)
          </el-form-item>

          <el-form-item label="虚拟卡内容">
            <el-input v-model="addForm.content" clearable type="textarea" :rows="6" />
          </el-form-item>

          <el-form-item label="检查重复卡">
            <el-radio-group v-model="addForm.range">
              <el-radio :label="1">是</el-radio>
              <el-radio :label="2">否</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item>
            <el-button type="primary">确定</el-button>
          </el-form-item>
        </el-form>
      </el-tab-pane>
      <el-tab-pane label="导入" name="export">
        <el-form class="add-form" ref="form" :model="addForm" label-width="120px">
          <el-form-item label="指定商品">
            <el-select v-model="addForm.readState" style="width: 100%" clearable placeholder="商品类型">
              <el-option label="掘地求生" :value="1"></el-option>
            </el-select>
          </el-form-item>

          <el-form-item label="选择文件">
            <el-upload :limit="1" drag action="https://jsonplaceholder.typicode.com/posts/">
              <i class="el-icon-upload"></i>
              <div class="el-upload__text">
                将文件拖到此处，或
                <em>点击上传</em>
              </div>
              <div class="el-upload__tip" slot="tip">只能上传txt文件，且不超过500kb</div>
            </el-upload>
          </el-form-item>

          <el-form-item label="检查重复卡">
            <el-radio-group v-model="addForm.range">
              <el-radio :label="1">是</el-radio>
              <el-radio :label="2">否</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item>
            <el-button type="primary">导入</el-button>
          </el-form-item>
        </el-form>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>
<script>
export default {
  data() {
    return {
      activeName: "add",
      addForm: {},
    };
  },
};
</script>
<style scoped>
.dialog >>> .el-form {
  padding: 0;
}
.add-form {
  width: 500px;
}
</style>