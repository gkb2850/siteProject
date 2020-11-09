<template>
  <div class="dashboard-container">
    <!-- 卡片列表 -->
    <div class="pan-card-container">
      <el-card shadow="hover" class="pan-card">
        <div class="card-icon">
          <img src="@/assets/icon/h1.jpg" alt />
        </div>
        <div class="card-meta">
          <div class="card-money">100.20元</div>
          <div class="card-title">账户余额</div>
          <div class="card-remark">满100自动结算，无须手动提现</div>
        </div>
      </el-card>
      <el-card shadow="hover" class="pan-card">
        <div class="card-icon">
          <img src="@/assets/icon/h2.jpg" alt />
        </div>
        <div class="card-meta">
          <div class="card-money">100.20元</div>
          <div class="card-title">今日收入</div>
          <div class="card-remark">
            今日成交：0笔
            <br />今日卖出：0张
          </div>
        </div>
      </el-card>
      <el-card shadow="hover" class="pan-card">
        <div class="card-icon">
          <img src="@/assets/icon/h1.jpg" alt />
        </div>
        <div class="card-meta">
          <div class="card-money">100.20元</div>
          <div class="card-title">冻结金额</div>
          <div class="card-remark">
            今日利润：0.00
            <br />昨日订单：0比
          </div>
        </div>
      </el-card>
      <el-card shadow="hover" class="pan-card">
        <div class="card-icon">
          <img src="@/assets/icon/h2.jpg" alt />
        </div>
        <div class="card-meta">
          <div class="card-money">100.20元</div>
          <div class="card-title">最近提现</div>
          <div class="card-remark">最近结算：100.20元</div>
        </div>
      </el-card>
    </div>
    <!-- 卡片列表结束 -->
    <!-- 统计图 -->
    <div class="order-stastic">
      <el-card shadow="never" class="stastic-card">
        <div slot="header" class="clearfix">
          <span>近期交易统计</span>
        </div>
        <div class="stastic-content" id="container"></div>
      </el-card>
    </div>
    <!-- 统计图 -->
    <el-row :gutter="20" class="bottom-table-container">
      <!-- 系统公告 -->
      <el-col :span="8">
        <el-card shadow="never" class="bottom-card card-left">
          <div slot="header" class="clearfix">
            <span>系统公告</span>
          </div>
          <div class="bottom-content"></div>
        </el-card>
      </el-col>
      <!-- 系统公告 -->
      <!-- 最新订单 -->

      <el-col :span="16">
        <el-card shadow="never" class="bottom-card card-right">
          <div slot="header" class="clearfix">
            <span>最新订单信息</span>
          </div>
          <div class="bottom-content">
            <el-table
              stripe
              :data="newOrderList"
              header-row-class-name="faka-table-header"
              style="width: 100%"
              v-loading="loading"
            >
              <el-table-column prop="systemNoticeTitle" label="订单号"></el-table-column>
              <el-table-column prop="systemNoticeTitle" label="商品名称"></el-table-column>
              <el-table-column prop="systemNoticeTitle" label="交易金额"></el-table-column>
              <el-table-column prop="systemNoticeTitle" label="下单时间"></el-table-column>
              <el-table-column prop="systemNoticeTitle" label="状态"></el-table-column>
              <el-table-column prop="systemNoticeTitle" label="支付通道"></el-table-column>
            </el-table>
          </div>
        </el-card>
      </el-col>
      <!-- 最新订单 -->
    </el-row>
  </div>
</template>

<script>
import { Chart } from "@antv/g2";
export default {
  name: "DashboardAdmin",
  components: {},
  data() {
    return {
      newOrderList: [],
      loading: false,
      chartData: [
        { day: "Jan", types: "Tokyo", value: 7 },
        { day: "Jan", types: "London", value: 3.9 },
        { day: "Feb", types: "Tokyo", value: 6.9 },
        { day: "Feb", types: "London", value: 4.2 },
        { day: "Mar", types: "Tokyo", value: 9.5 },
        { day: "Mar", types: "London", value: 5.7 },
        { day: "Apr", types: "Tokyo", value: 14.5 },
        { day: "Apr", types: "London", value: 8.5 },
        { day: "May", types: "Tokyo", value: 18.4 },
        { day: "May", types: "London", value: 11.9 },
        { day: "Jun", types: "Tokyo", value: 21.5 },
        { day: "Jun", types: "London", value: 15.2 },
        { day: "Jul", types: "Tokyo", value: 25.2 },
        { day: "Jul", types: "London", value: 17 },
        { day: "Aug", types: "Tokyo", value: 26.5 },
        { day: "Aug", types: "London", value: 16.6 },
        { day: "Sep", types: "Tokyo", value: 23.3 },
        { day: "Sep", types: "London", value: 14.2 },
        { day: "Oct", types: "Tokyo", value: 18.3 },
        { day: "Oct", types: "London", value: 10.3 },
        { day: "Nov", types: "Tokyo", value: 13.9 },
        { day: "Nov", types: "London", value: 6.6 },
        { day: "Dec", types: "Tokyo", value: 9.6 },
        { day: "Dec", types: "London", value: 4.8 },
      ],
      chart: null
    };
  },
  mounted() {
    this.$nextTick(()=>{
      this.initChart()
    this.getStasticData();
    })
  },
  methods: {
    initChart() {
      this.chart = new Chart({
        container: "container",
        autoFit: true,
        height: 500,
      });
    },
    getStasticData() {
      this.chart.data(this.chartData);
      this.chart.scale({
        day: {
          range: [0, 1],
        },
        value: {
          nice: true,
        },
      });

      this.chart.tooltip({
        showCrosshairs: true,
        shared: true,
      });

      this.chart.axis("value", {
        label: {
          formatter: (val) => {
            return val + " °C";
          },
        },
      });

      this.chart.line().position("day*value").color("types").shape("smooth");

      this.chart.point().position("day*value").color("types").shape("circle");

      this.chart.render();
    },
  },
};
</script>

<style scoped>
.dashboard-container {
  padding: 20px;
}
.pan-card-container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.pan-card {
  width: 23%;
}
.pan-card >>> .el-card__body {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
.card-icon {
  width: 100px;
}
.card-icon img {
  width: 100px;
  height: 100px;
}

.card-meta {
  text-align: right;
}

.card-money {
  font-size: 30px;
}

.card-remark {
  font-size: 14px;
  margin-top: 10px;
  color: #a5a5a5;
}

.bottom-table-container {
  margin-top: 15px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.order-stastic {
  margin-top: 15px;
}
.stastic-content {
  min-height: 300px;
  max-height: 500px;
}
</style>
