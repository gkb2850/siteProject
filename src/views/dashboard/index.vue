<template>
  <div>


              <div class="dashboard-container">
              <div class="section section2" style="width:100%;border:1px solid #eaeaea">
                <div class="title">今日统计</div>
                <div class="record-warp">
                    <div class="item" v-for="(item,index) in records" :key="index">
                        <div class="content">
                           <div class="info">
                              <p class="name">{{item.name}}</p>
                              <p>{{item.num}} {{item.tips}}</p>
                           </div>
                           <div>
                              <img :src="item.icon" />
                           </div>
                        </div>
                    </div>
                </div>
              </div>
        </div>
    
      <div :class="isWidth ? 'dashboard-container-true' : 'dashboard-container'">
        <div :class="isWidth ? 'el-left-div-content-true' : 'el-left-div-content'">
              <div class="section section1">
                <div class="title">产品更新<span class="more">更多</span></div>
                <!-- <ul>
                  <li>
                    <a href="" style="color:#FF0000">关于5月份和6月份更新内容</a>
                    <span class="date">2020-06-10</span>
                  </li>
                  <li>
                    <a href="" style="color:#FF00FF">新卡售2.0即将上线</a>
                    <span class="date">2020-06-10</span>
                  </li>
                </ul> -->
                  <el-table :data="tableData" style="width: 100%" :show-header="status">
                       <el-table-column :show-overflow-tooltip="true" prop="content" label="名称">
                         <template slot-scope="scope">
                            <img src="index_news.png" style="margin-right:5px">
                            <a @click="goNotice(scope.row.updateNoticeID)" target="_blank"><span :style="{'color':scope.row.color}">{{scope.row.title}}</span></a>
                         </template>
                      </el-table-column>
                       <el-table-column :show-overflow-tooltip="true" prop="createTime" label="时间" width="180">
                         <template slot-scope="scope">
                            <span>{{scope.row.createTime}}</span>
                         </template>
                      </el-table-column>
                  </el-table>
              </div>
        </div>
        <div :class="isWidth ? 'el-right-div-content-true' : 'el-right-div-content'">
              <div class="section section2">
                <div class="title">商户信息</div>
                <ul>
                  <li v-for="(item,i) in LoginSiteInfo" style="margin:10px 0;display:flex;align-items: flex-start;">
                    <span class="label" style="font-size: 14px;font-weight: 600;">{{item.key}}：</span>
                    <div style="margin-top:1px;">
                      <span style="margin-left:10px;display:block" class="val"v-for="(items,is) in item.value" >{{items}}</span>
                    </div>
                  </li>
                  <!-- <li>
                    <span class="label">网站域名：</span>
                    <span class="val">www.sdfsdf.com</span>
                  </li>
                  <li>
                    <span class="label">网站版本：</span>
                    <span class="val"><el-button type="danger" size="mini">旗舰版</el-button></span>
                  </li>
                  <li>
                    <span class="label">到期时间：</span>
                    <span class="val">2018-06-04 至 2020-06-29</span>
                  </li>
                  <li>
                    <span class="label">实名认证状态：</span>
                    <span class="val"><el-button type="primary" plain="" size="mini">已实名</el-button></span>
                  </li> -->
                </ul>
              </div>
        </div>
        
      </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import { getLists,getDatas } from '@/api/price/price-1/common'

export default {
  name: 'Dashboard',
  data() {
    return {
      status:false,
      form:{
        A:0,
        B:0,
        C:0,
        D:0,
        E:0,
        F:0,
      },
      isWidth: false,
      tableData:[],
      LoginSiteInfo:[],
      records:[
        {
          icon:require('../../assets/icon/h1.jpg'),
          name:'充值金额',
          num:'0',
          tips:'元'
        },
        {
          icon:require('../../assets/icon/h2.jpg'),
          name:'销售金额',
          num:'0',
          tips:'元'
        },
        {
          icon:require('../../assets/icon/h3.jpg'),
          name:'手续费',
          num:'0',
          tips:'元'
        },
        {
          icon:require('../../assets/icon/h4.jpg'),
          name:'直销订单',
          num:'0',
          tips:'个'
        },
        {
          icon:require('../../assets/icon/h5.jpg'),
          name:'对接订单',
          num:'0',
          tips:'个'
        },
        {
          icon:require('../../assets/icon/h6.jpg'),
          name:'投诉订单',
          num:'0',
          tips:'个'
        },
        {
          icon:require('../../assets/icon/h7.jpg'),
          name:'注册用户',
          num:'0',
          tips:'个'
        },
        {
          icon:require('../../assets/icon/h8.jpg'),
          name:'搭建子站',
          num:'0',
          tips:'个'
        }
      ]
    }
  },
  computed: {
    ...mapGetters([
      'name'
    ])
  },
  created(){
    var w=document.documentElement?document.documentElement.clientWidth:document.body.clientWidth;//先获取窗口宽度
    console.log(w)
    if(w<750){
      if (!sessionStorage.getItem('loaded')) {
        sessionStorage.setItem('loaded', true)
        location.reload()
      }
      this.isWidth = true
    }
    this.getLists()
    this.getOrderToday()
    this.getPayMoneyToday()
    this.getOrderMoneyToday()
    this.getMoneyServiceToday()
    this.getRegToday()
    this.getComplaintToday()
    this.getSiteToday()
    this.getLoginSite()
  },

  methods: {

    // 获取公告信息
    getLists(){
      getLists({pageNum:1,pageSize:40},'/platform/updateNotice/page').then(res => {
        if (res.code===1001){
          this.tableData = res.body.records
        }
      }).catch(err => {
      })
    },

    // 统计--获取
    
    //获取当日订单数
    getOrderToday(){
      //直销
      getDatas({saleType:1},'/order/order/orderToday').then(res => {
        if (res.code===1001){
          this.records[3].num = res.body
        }
      }).catch(err => {
      }),
      //对接
      getDatas({saleType:2},'/order/order/orderToday').then(res => {
        if (res.code===1001){
          this.records[4].num = res.body
        }
      }).catch(err => {
      })
    },
    //获取当日销售金额
    getOrderMoneyToday(){
      getDatas({},'/order/order/orderMoneyToday').then(res => {
        if (res.code===1001){
           this.records[1].num = res.body
        }
      }).catch(err => {
      })
    },
    //获取当日充值金额
    getPayMoneyToday(){
      getDatas({},'/finance/moneyStatistics/moneyRechargeToday').then(res => {
        if (res.code===1001){
          this.records[0].num = res.body
        }
      }).catch(err => {
      })
    },
    //获取当日手续费
    getMoneyServiceToday(){
      getDatas({},'/finance/moneyStatistics/moneyServiceToday').then(res => {
        if (res.code===1001){
          this.records[2].num = res.body
        }
      }).catch(err => {
      })
    },

    //获取当日注册用户
    getRegToday(){
      getDatas({},'/user/user/regToday').then(res => {
        if (res.code===1001){
          this.records[6].num = res.body
        }
      }).catch(err => {
      })
    },

    //获取当日投诉数
    getComplaintToday(){
      getDatas({},'/order/complaint/complaintToday').then(res => {
        if (res.code===1001){
          this.records[5].num = res.body
        }
      }).catch(err => {
      })
    },

    //获取当日搭建子站数
    getSiteToday(){
      getDatas({},'/platform/siteStatistics/siteToday').then(res => {
        if (res.code===1001){
          this.records[7].num = res.body
        }
      }).catch(err => {
      })
    },

    // 商户信息
    getLoginSite(){
      getDatas({},'/platform/site/getLoginSite').then(res => {
        if (res.code===1001){
          this.LoginSiteInfo = res.body
          //console.log(this.LoginSiteInfo,111)
        }
      }).catch(err => {
      })
    },

    // 跳转详情
    goNotice(id){
      this.$router.push({ path:'/menu1/goNotice/'+id})
    },




  },
}
</script>

<style lang="scss" scoped>

.el-table__header-wrapper {
    display:none !important;
}

.dashboard-container-true{
  .el-left-div-content-true{
      width: 100%;
      border: 1px solid #eaeaea;
  }
  .el-right-div-content-true{
    width: 100%;
    border: 1px solid #eaeaea;
    margin-top: 20px;
  }
  padding:20px;
  .title {
    display:flex;
    justify-content: space-between;

    .more {
      font-weight: normal;
    }
  }
  ul {
      margin:0;
      padding:0;
    }
  .section {
    margin-bottom:30px;
  }
  .section1 {
    
    li {
      font-size:13px;
      padding:5px 0;
      cursor: pointer;
      display:flex;
      justify-content: space-between;
      padding:5px 20px;
      margin:0;
      a {
        font-size:12px;
      }
      .date {
        color:#999;
      }
    }
  }
  .section2 {
    li {
      display:flex;
      font-size:13px;
      color:#888;
      padding:5px 0;
      align-items: center;
      .label {
        width:140px;
        text-align: right;
        
      }
    }
  }
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}




.dashboard-container {
  display:flex;
  .el-left-div-content{
    width: calc(50% - 10px);
    border: 1px solid #eaeaea;
    .title{
      margin-bottom: 5px;
    }
    span{
      color: #888;
      font-size: 13px;
    }
  }
  .el-right-div-content{
    width: calc(50% - 10px);
    margin: 0 0 0 20px;
    border: 1px solid #eaeaea;
    li{
      margin: 5px 0;
      padding: 3px 0!important;
    }
    .label{
      font-weight: normal!important;
    }
  }

  padding:20px 20px 0 20px;
  .title {
    display:flex;
    justify-content: space-between;
    .more {
      font-weight: normal;
    }
  }
  ul {
      margin:0;
      padding:0;
    }
  
  .section1 {
    
    li {
      font-size:13px;
      padding:5px 0;
      cursor: pointer;
      display:flex;
      justify-content: space-between;
      padding:5px 80px;
      margin:0;
      a {
        font-size:12px;
      }
      .date {
        color:#999;
      }
    }
  }
  .section2 {
    li {
      display:flex;
      font-size:13px;
      color:#888;
      padding:5px 0;
      align-items: center;
      .label {
        width:140px;
        text-align: right;
        
      }
    }
  }
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
  

}


  .dashboard-container-r{
    width: 50%;
    float:right;
    .el-title-p-wl{
      line-height:6px;
      color:#898989;
    }
  }
  .el-title-sp-wl{
    color: #d07100;
    font-weight: bold;
  }


  .wl-box-card{
    display: flex;
  }
  .wl-box-card-right{
    min-width:60%;
  }
  .wl-box-card-left{
    height:100px;
    width:40%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .wl-info-content{
    line-height:50px;
    margin: 0;
    padding-left: 20px;
  }
  .wl-info-title{
    line-height:50px;
    margin: 0;
    padding-left: 20px;
    font-weight: 500;
  }
  .user-avatar{
    width: 40px;
    height:40px;
  }

.row-bg{
   .el-col-6{
    padding: 0 20px!important;
  }
}

 
</style>
