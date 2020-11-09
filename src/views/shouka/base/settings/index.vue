<template>
  <div>
    <el-card shadow="never">
      <el-col :span="12">
        <div class="form-div">
          <div class="header">商户基本信息</div>
          <div class="content">
            <el-form ref="form" :model="form" label-width="120px">
              <el-form-item label="商户编号">10443</el-form-item>
              <el-form-item label="支付接口秘钥">Yr7aivzZwKdrIWXU3fte</el-form-item>
              <el-form-item label="用户名">16456616</el-form-item>
              <el-form-item label="电子邮箱">66464@qq.com</el-form-item>
              <el-form-item label="手机号码">
                <el-input v-model="form.phone"></el-input>
              </el-form-item>
              <el-form-item>
                <el-button type="primary">获取验证码</el-button>
              </el-form-item>
              <el-form-item label="短信验证码">
                <el-input v-model="form.random"></el-input>
                <div class="danger-text">修改手机号需验证旧手机号码，新增或不修改，此项留空，验证码错误时，手机号码不会修改</div>
              </el-form-item>
              <el-form-item></el-form-item>
              <el-form-item label="安全验证">
                <el-radio-group v-model="form.check">
                  <el-radio :label="3">支持</el-radio>
                  <el-radio :label="6">不支持</el-radio>
                </el-radio-group>
                <div class="danger-text">请设置6位安全码，推荐建议勾选</div>
              </el-form-item>
              <el-form-item label="商户QQ">
                <el-input v-model="form.qqrandom"></el-input>
              </el-form-item>

              <el-form-item label="商户网站">
                <el-input v-model="form.random"></el-input>
                <div class="info-text">网址前面加http://或https://</div>
              </el-form-item>

              <el-form-item label="子域名">
                <el-input v-model="form.random"></el-input>
                <div class="info-text">请设置您店铺地址，例如：shouka /请不要填写http 或https</div>
              </el-form-item>

              <el-form-item label="店铺名称">
                <el-input v-model="form.random"></el-input>
              </el-form-item>

              <el-form-item label="店铺公告">
                <el-input v-model="form.random" :rows="4" type="textarea"></el-input>
              </el-form-item>

              <el-form-item label="系统公告自动弹出">
                <el-select v-model="form.region" style="width: 100%">
                  <el-option label="是" value="1"></el-option>
                  <el-option label="否" value="0"></el-option>
                </el-select>
              </el-form-item>

              <el-form-item label="商家自动弹出">
                <el-select v-model="form.region" style="width: 100%">
                  <el-option label="是" value="1"></el-option>
                  <el-option label="否" value="0"></el-option>
                </el-select>
              </el-form-item>

              <el-form-item label="购卡协议自动弹出">
                <el-select v-model="form.region" style="width: 100%">
                  <el-option label="是" value="1"></el-option>
                  <el-option label="否" value="0"></el-option>
                </el-select>
              </el-form-item>

              <el-form-item label="支付页面风格">
                <el-select v-model="form.region" style="width: 100%">
                  <el-option label="绝地求生" value="1"></el-option>
                  <el-option label="初音未来" value="0"></el-option>
                </el-select>
              </el-form-item>

              <el-form-item label="库存显示方式">
                <el-select v-model="form.stockShowType" style="width: 100%">
                  <el-option label="实际库存" :value="1"></el-option>
                  <el-option label="库存范围" :value="0"></el-option>
                </el-select>
                <div v-show="form.stockShowType === 0">
                  1.库存大于100，显示 库存非常多
                  <br />2.库存小于100、大于30，显示 库存很多
                  <br />3.库存小于30、大于10，显示 库存一般
                  <br />4.库存小于10，显示 库存少量
                </div>
              </el-form-item>
              <el-form-item label="提现方式">
                <el-select v-model="form.region" style="width: 100%">
                  <el-option label="系统默认" value="1"></el-option>
                  <el-option label="手动提现" value="0"></el-option>
                  <el-option label="自动提现" value="0"></el-option>
                </el-select>
                <div class="danger-text">手工提现：手动申请提现。自动提现：金额满 100 元系统自动生成提款记录无需手工操作。</div>
              </el-form-item>

              <el-form-item label="费率承担方">
                <el-select v-model="form.region" style="width: 100%">
                  <el-option label="跟随系统" value="1"></el-option>
                  <el-option label="卖家承担" value="0"></el-option>
                  <el-option label="买家承担" value="0"></el-option>
                </el-select>
              </el-form-item>

              <el-form-item label="代理商对接码">
                <el-input value="Uf8eAxUAcj8bBiVfjks2" :disabled="true"></el-input>
              </el-form-item>
              <el-form-item label="代理审核方式">
                <el-select v-model="form.region" style="width: 100%">
                  <el-option label="手动" value="1"></el-option>
                  <el-option label="自动" value="0"></el-option>
                </el-select>
              </el-form-item>

              <el-form-item label="绑定微信">
                <vue-qr :text="qrCode" :size="150"></vue-qr>
              </el-form-item>

              <el-form-item>
                <el-button type="primary">保存设置</el-button>
              </el-form-item>
            </el-form>
          </div>
        </div>
      </el-col>
      <el-col :span="12">
        <div class="form-div">
          <div class="header">商户收款信息</div>
          <div class="content">
            <el-form ref="form" :model="form" label-width="120px">
              <el-form-item label="收款方式">
                <el-select v-model="form.region" style="width: 100%">
                  <el-option label="支付宝收款" value="1"></el-option>
                </el-select>
              </el-form-item>

              <el-form-item label="支付宝账号">
                <el-input value="121515" :disabled="true" />
              </el-form-item>
              <el-form-item label="收款人姓名">
                <el-input value="咕咕咕" :disabled="true" />
              </el-form-item>
              <el-form-item label="身份证号">
                <el-input value="15314534354531" :disabled="true" />
              </el-form-item>


              <el-form-item label="收款二维码">
                <el-image src="http://img.1vhost.cn/5f3fbaed2552e/5f3fbaed2556b.jpg"></el-image>
              </el-form-item>
            </el-form>
          </div>
        </div>
      </el-col>
    </el-card>
  </div>
</template>

<script>
import VueQr from "vue-qr";
export default {
  data() {
    return {
      form: {},
      qrCode: "http://www.baidu.com",
    };
  },
  components: {
    VueQr,
  },
  methods: {},
};
</script>

<style scoped>
.form-div {
  padding: 20px 100px;
}
.form-div .header {
  font-size: 18px;
  margin-bottom: 20px;
}

.form-div .content {
  width: 70%;
}

.form-div .el-form {
  padding: 0;
}

.danger-text {
  color: red;
}
</style>