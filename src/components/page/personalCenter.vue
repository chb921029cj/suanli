<template>
    <div class="personalCenter">
        <div class="container">
            <div class="row">
                <div class="col-12 col-lg-3">
                    <div class="row align-items-center header">
                        <div class="col-4 col-lg-12 text-center ">
                            <i class="el-icon-service logo"></i>
                        </div>
                        <div class="col-8 col-lg-12 tel">
                            <p>{{this.userInfo.username}}</p>
                            <span><i class="el-icon-service "></i>{{this.userInfo.username}}</span>
                            <span><i class="el-icon-service "></i>{{this.userInfo.email}}</span>
                        </div>
                        <div class="col-12 time">
                            上次登录时间：	{{this.userInfo.lip}}
                        </div>
                        <div class="col-12 ip">
                            上次登录IP：	{{this.userInfo.ltime|dateServer}}
                        </div> 
                        <div class="col-12 start">
                            <div class="row">
                                <div class="col-4 startss">账户安全等级</div>
                                <div class="col-8 starts"><el-rate v-model="start"></el-rate></div>
                                <p  class="col-12">建议尽快完善相关信息，提高账户安全性</p>
                            </div>
                        </div>
                        
                    </div>
                    <div class="row d-none d-lg-block mt-4  ">
                        <el-menu
                                default-active="property"
                                 background-color="#545c64"
                                class="el-menu-vertical-demo w-100 nav-vertical-demo"
                             @select="handleSelect"
                                  text-color="#fff"
                                 active-text-color="#d86f06">
                                <el-menu-item index="property">
                                  <i class="el-icon-menu"></i>
                                  <span slot="title">资产总览</span>
                                </el-menu-item>                            
                                <el-menu-item index="hashrate">
                                  <i class="el-icon-menu"></i>
                                  <span slot="title">算力租赁</span>
                                </el-menu-item>
                                <el-menu-item index="mill">
                                  <i class="el-icon-document"></i>
                                  <span slot="title">矿机租赁</span>
                                </el-menu-item>
                                <el-menu-item index="millRoommates">
                                  <i class="el-icon-setting"></i>
                                  <span slot="title">矿机合租</span>
                                </el-menu-item>
                                <el-menu-item index="5">
                                  <i class="el-icon-setting"></i>
                                  <span slot="title">商城订单</span>
                                </el-menu-item>
                                <el-menu-item index="pCenter">
                                  <i class="el-icon-setting"></i>
                                  <span slot="title">个人中心</span>
                                </el-menu-item>
                                <el-menu-item index="recharge">
                                  <i class="el-icon-setting"></i>
                                  <span slot="title">我要充值</span>
                                </el-menu-item>
                                <el-menu-item index="asset">
                                  <i class="el-icon-setting"></i>
                                  <span slot="title">提取资产</span>
                                </el-menu-item> 
                               <el-menu-item index="billing">
                                  <i class="el-icon-setting"></i>
                                  <span slot="title">账单明细</span>
                                </el-menu-item>                                                                                                                                            
                        </el-menu>
                    </div>
                    <div class="row  d-lg-none mt-4">
                        <el-menu default-active="property" class="el-menu-demo" mode="horizontal" @select="handleSelect">
                                    <el-menu-item index="property">
                                      资产总览
                                    </el-menu-item>                            
                                    <el-menu-item index="hashrate">
                                     算力租赁
                                    </el-menu-item>
                                    <el-menu-item index="mill">
                                      矿机租赁
                                    </el-menu-item>
                                    <el-menu-item index="millRoommates">
                                    矿机合租
                                    </el-menu-item>
                                    <el-menu-item index="5">
                                    商城订单
                                    </el-menu-item>
                                    <el-menu-item index="pCenter">
                                    个人中心
                                    </el-menu-item>
                                    <el-menu-item index="recharge">
                                    我要充值
                                    </el-menu-item>
                                    <el-menu-item index="asset">
                                    提取资产
                                    </el-menu-item> 
                                   <el-menu-item index="billing">
                                    账单明细
                                    </el-menu-item>                
                        </el-menu>
                    </div>

                </div>
                <div class="col-12 col-lg-9">
                    <router-view></router-view>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import vSlider from "../slider/slider";
import { animate } from "@/config/animate";
import { mapState, mapGetters, mapMutations } from "vuex";
export default {
  data() {
    return {
      start: 3.7
    };
  },
  components: {
    vSlider
  },
  computed: {
    ...mapGetters(["userInfo", "isLogin"])
  },
  methods: {
    handleOpen(key, keyPath) {
      console.log(key, keyPath);
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath);
    },
    handleSelect(key, keyPath) {
      this.$router.push(key);
    },
    //返回顶部
    backTop() {
      animate(document.body, { scrollTop: "0" }, 400, "ease-out");
    }
  },

  beforeUpdate: function() {},
  updated: function() {
    this.backTop();
  },
  mounted() {
    this.backTop();
  }
};
</script>
<style lang="less" scoped>
.personalCenter {
  margin-bottom: 20px;
  .logo {
    font-size: 72px;
  }
  .header {
    margin-top: 30px;
    background: #fff;
    padding: 20px 0;
  }
  .tel {
    p {
      height: 35px;
      line-height: 35px;
      font-size: 18px;

      margin-bottom: 5px;
      white-space: nowrap;
      overflow: hidden;
      @media screen and (max-width: 720px) {
        & {
          padding-left: 15px;
        }
      }
      @media screen and (min-width: 721px) {
        & {
          padding-left: 75px;
        }
      }
    }
    span {
      display: inline-block;
      height: 30px;
      line-height: 30px;
      color: #666;
      i {
        margin-right: 10px;
      }
      @media screen and (max-width: 720px) {
        & {
          padding-left: 15px;
        }
      }
      @media screen and (min-width: 721px) {
        & {
          padding-left: 70px;
        }
      }
    }
  }
  .time,
  .ip {
    font-size: 12px;
    color: #888;
    line-height: 24px;
    padding-left: 20px;
  }
  .start {
    font-size: 13px;
    line-height: 19px;
    margin-top: 4px;
    padding-left: 20px;
    @media screen and (max-width: 720px) {
      & {
        color: #888;
        border-top: 1px solid #eee;
        margin-top: 11px;
        padding-top: 10px;
      }
    }
    @media screen and (min-width: 721px) {
      & {
      }
    }
    .startss {
      padding-right: 0;
    }
    .starts {
      padding: 0;
    }
    p {
      font-size: 12px;
      line-height: 16px;
      color: #999;
      margin-top: 8px;
    }
  }
  .navs {
  }
  .nav-vertical-demo {
    font-weight: bold;
  }
}
</style>

