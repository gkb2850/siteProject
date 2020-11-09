<template>
  <div class="dialog">
    <el-card shadow="never">
      <el-form class="add-form" ref="form" :model="addForm" label-width="120px">
        <el-form-item label="商品分类">
          <div class="item">
            <div class="item-form">
              <el-select
                v-model="addForm.readState"
                style="width: 100%"
                clearable
                placeholder="商品分类"
              >
                <el-option label="卡盟加款机器人" :value="2"></el-option>
              </el-select>
            </div>
            <div class="item-info">请选择商品分类</div>
          </div>
        </el-form-item>
        <el-form-item label="页面风格">
          <div class="item">
            <div class="item-form">
              <el-select
                v-model="addForm.readState"
                style="width: 100%"
                clearable
                placeholder="页面风格"
              >
                <el-option label="默认风格" :value="2"></el-option>
              </el-select>
            </div>
            <div class="item-info">请选择购买时的页面风格</div>
          </div>
        </el-form-item>
        <el-form-item label="商品排序">
          <div class="item">
            <div class="item-form">
              <el-input-number v-model="addForm.content" />
            </div>
            <div class="item-info">数字越大越靠前！</div>
          </div>
        </el-form-item>
        <el-form-item label="商品名称">
          <div class="item">
            <div class="item-form">
              <el-input v-model="addForm.content" clearable />
            </div>
            <div class="item-info">好的名字有利于销售哦！</div>
          </div>
        </el-form-item>
        <el-form-item label="商品价格">
          <div class="item">
            <div class="item-form">
              <el-input v-model="addForm.content" clearable />
            </div>
            <div class="item-info">商品对外出售的价格即零售价格！</div>
          </div>
        </el-form-item>

        <el-form-item label="批发优惠">
          <div class="item">
            <div class="item-form">
              <el-radio-group v-model="addForm.discount">
                <el-radio :label="1">使用</el-radio>
                <el-radio :label="2">不使用</el-radio>
              </el-radio-group>
            </div>
            <div class="item-info">是否设置批发购买时使用批发价格！</div>
          </div>
        </el-form-item>

        <div v-show="addForm.discount === 1">
          <el-form-item v-for="(discount) in addForm.discounts" :key="discount.key">
            <el-input-number v-model="discount.count" style="width: 30%" />&nbsp;&nbsp;张
            <el-input v-model="discount.money" style="width: 30%"></el-input>&nbsp;&nbsp;元
            <el-button @click.prevent="removeDiscount(discount)">删除</el-button>
          </el-form-item>

          <el-form-item>
            <div class="item">
              <div class="item-form">
                <el-button type="primary" @click="addDiscount">添加优惠</el-button>
              </div>
              <div class="item-info">优惠价格为单价，并不是商品总价哦！</div>
            </div>
          </el-form-item>
        </div>

        <el-form-item label="成本价格">
          <div class="item">
            <div class="item-form">
              <el-input v-model="addForm.content" placeholder="单位（元）" clearable />
            </div>
            <div class="item-info">商品进货价,可以不填,填写有利于商户系统的利润统计分析！</div>
          </div>
        </el-form-item>

        
        <el-form-item label="短信费用">
              <el-radio-group v-model="addForm.discount">
                <el-radio :label="1">买家承担</el-radio>
                <el-radio :label="2">卖家承担</el-radio>
              </el-radio-group>
        </el-form-item>

        <el-form-item label="起购数量">
          <div class="item">
            <div class="item-form">
              <el-input-number :min="1" v-model="addForm.content" />
            </div>
            <div class="item-info">每次购买 最少购买多少件！</div>
          </div>
        </el-form-item>

        <el-form-item label="库存预警">
          <div class="item">
            <div class="item-form">
              <el-input-number :min="0" v-model="addForm.content" />
            </div>
            <div class="item-info">为0表示不报警,设置后请确保邮箱正确，否则无法接收邮件！</div>
          </div>
        </el-form-item>

        <el-form-item label="通知方式">
          <el-radio-group v-model="addForm.aa">
            <el-radio :label="1">站内信</el-radio>
            <el-radio :label="2">邮件</el-radio>
          </el-radio-group>
        </el-form-item>

        <el-form-item label="出卡方式">
          <el-radio-group v-model="addForm.aa">
            <el-radio :label="1">顺序出卡</el-radio>
            <el-radio :label="2">随机出卡</el-radio>
          </el-radio-group>
        </el-form-item>

        <el-form-item label="优惠券">
          <div class="item">
            <div class="item-form">
              <el-radio-group v-model="addForm.aa">
                <el-radio :label="1">支持</el-radio>
                <el-radio :label="2">不支持</el-radio>
              </el-radio-group>
            </div>
            <div class="item-info">是否支持优惠券</div>
          </div>
        </el-form-item>

        <el-form-item label="售出通知">
          <div class="item">
            <div class="item-form">
              <el-radio-group v-model="addForm.aa">
                <el-radio :label="1">开启</el-radio>
                <el-radio :label="2">关闭</el-radio>
              </el-radio-group>
            </div>
            <div class="item-info">开启后，成功售卡将会发送邮件通知</div>
          </div>
        </el-form-item>

        <el-form-item label="提卡密码">
          <div class="item">
            <div class="item-form">
              <el-radio-group v-model="addForm.aa">
                <el-radio :label="1">必填</el-radio>
                <el-radio :label="2">选填</el-radio>
                <el-radio :label="3">关闭</el-radio>
              </el-radio-group>
            </div>
            <div class="item-info">开启后，购买页面会提示买家填写取卡密码</div>
          </div>
        </el-form-item>

        <el-form-item label="访问密码">
          <div class="item">
            <div class="item-form">
              <el-radio-group v-model="addForm.password">
                <el-radio :label="1">开启</el-radio>
                <el-radio :label="2">关闭</el-radio>
              </el-radio-group>
            </div>
            <div class="item-info">留空为不开启，若开启后商品购买页面将提示输入设置的密码才能继续访问</div>
          </div>
        </el-form-item>

        <el-form-item label="设置密码" v-show="addForm.password === 1">
          <div class="item">
            <div class="item-form">
              <el-input v-model="addForm.aaaa" placeholder="请输入密码" clearable />
            </div>
            <div class="item-info">设置密码后，请牢记该密码！</div>
          </div>
        </el-form-item>

        <el-form-item label="代理销售">
          <div class="item">
            <div class="item-form">
              <el-radio-group v-model="addForm.proxy">
                <el-radio :label="1">支持</el-radio>
                <el-radio :label="2">不支持</el-radio>
              </el-radio-group>
            </div>
            <div class="item-info">是否代理销售该商品,如若支持,则生成授权码,下发给代理商户即可实现代理销售</div>
          </div>
        </el-form-item>

        <div v-show="addForm.proxy === 1">
          <el-form-item label="代理价格">
            <div class="item">
              <div class="item-form">
                <el-input v-model="addForm.aaaa" clearable />
              </div>
              <div class="item-info">商品给代理销售的价格！价格不能为0</div>
            </div>
          </el-form-item>
          <el-form-item label="代理加价">
            <div class="item">
              <div class="item-form">
                <el-input v-model="addForm.aaaa" clearable />
              </div>
              <div class="item-info">代理加价后才能销售！代理加价要大于 0.1 以上</div>
            </div>
          </el-form-item>
        </div>

        <el-form-item label="客户信息">
          <div class="item">
            <div class="item-form">
              <el-select
                v-model="addForm.readState"
                style="width: 100%"
                clearable
                placeholder="页面风格"
              >
                <el-option label="默认" :value="2"></el-option>
                <el-option label="任意字符" :value="2"></el-option>
                <el-option label="QQ号码" :value="2"></el-option>
                <el-option label="邮箱账号" :value="2"></el-option>
                <el-option label="手机" :value="2"></el-option>
              </el-select>
            </div>
            <div class="item-info">客户下单时输入的联系方式或充值账号格式限制</div>
          </div>
        </el-form-item>

        <el-form-item label="商品说明">
          <div class="item">
            <div class="item-form">
              <el-input
                maxlength="200"
                show-word-limit
                v-model="addForm.comment"
                clearable
                placeholder="建议填写该商品的使用方法，文字不超过200字"
                type="textarea"
                :rows="6"
              />
            </div>
            <div class="item-info">商品说明将显示在商品购买页面</div>
          </div>
        </el-form-item>

        <el-form-item label="使用说明">
          <div class="item">
            <div class="item-form">
              <el-input
                maxlength="200"
                show-word-limit
                v-model="addForm.comment2"
                clearable
                placeholder="建议填写该商品的使用方法，文字不超过200字"
                type="textarea"
                :rows="6"
              />
            </div>
            <div class="item-info">使用说明将显示在订单查询结果中，一般设置售后QQ群，或者下载地址类</div>
          </div>
        </el-form-item>

        <el-form-item>
          <el-button type="primary">确定提交</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>
<script>
export default {
  data() {
    return {
      addForm: {
        discounts: [],
      },
    };
  },
  methods: {
    removeDiscount(item) {
      var index = this.addForm.discounts.indexOf(item);
      if (index !== -1) {
        this.addForm.discounts.splice(index, 1);
      }
    },
    addDiscount() {
      this.addForm.discounts.push({
        count: "",
        money: "",
        key: Date.now(),
      });
    },
  },
};
</script>
<style scoped>
.dialog >>> .el-form {
  padding: 0;
}
/* .add-form {
  width: 00px;
} */
.item {
  display: flex;
}

.item-form {
  width: 600px;
}

.item-info {
  margin-left: 20px;
}
</style>