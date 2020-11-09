<template>
<div class="login-container">
    <canvas id="Mycanvas"></canvas>
    <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form" auto-complete="on" label-position="left">

        <div class="title-container">
            <img :src="loginLogo" />
        </div>

        <el-form-item prop="username">
            <el-input ref="username" v-model="loginForm.username" placeholder="用户名" style="width: '100%'" name="username" type="text" tabindex="1" auto-complete="on" />
        </el-form-item>

        <el-form-item prop="password">
            <el-input :key="passwordType" ref="password" v-model="loginForm.password" :type="passwordType" placeholder="密码" name="password" tabindex="2" auto-complete="on" @keyup.enter.native="handleLogin" />
        </el-form-item>
        <el-checkbox v-model="checked" style="margin-bottom: 10px;">记住密码</el-checkbox>
        <el-button :loading="loading" type="primary" style="width:100%;margin-bottom:30px;padding: 10px 10px;" @click.native.prevent="handleLogin">登录</el-button>
        <div class="third">
            <a :href="pageData.qq" target="_blank" ref="nofollow"><span class="qq"></span></a>
            <a :href="pageData.wx" target="_blank" ref="nofollow"><span class="wx"></span></a>
        </div>
        <!-- <div class="tips">
        <span style="margin-right:20px;">username: admin</span>
        <span> password: any</span>
      </div> -->

    </el-form>
</div>
</template>

<script>
import {constantRoutes, asyncRoutes} from '@/router'
import router from '@/router'
import {
    validUsername
} from '@/utils/validate'
import {
    login,
    logout,
    getInfo
} from '@/api/user'
import {
    getDatas
} from '@/api/price/price-1/common'
export default {
    name: 'Login',
    data() {
        const validateUsername = (rule, value, callback) => {
            //if (!validUsername(value)) {
            if (!value) {
                callback(new Error('请输入用户名'))
            } else {
                callback()
            }
        }
        const validatePassword = (rule, value, callback) => {
            if (value.length < 6) {
                callback(new Error('请输入大于6位的密码'))
            } else {
                callback()
            }
        }
        return {
            loginForm: {
                username: '',
                password: ''
            },
            checked: false,
            loginRules: {
                username: [{
                    required: true,
                    trigger: 'blur',
                    validator: validateUsername
                }],
                password: [{
                    required: true,
                    trigger: 'blur',
                    validator: validatePassword
                }]
            },
            loading: false,
            passwordType: 'password',
            redirect: undefined,
            pageData: {
                qq: '',
                wx: ''
            },
            loginLogo: ''
        }
    },
    activated() {
        // this.canvas()
    },
    mounted() {
        if (localStorage.getItem('user')) {
            this.loginForm.username = localStorage.getItem('user')
            this.loginForm.password = localStorage.getItem('secret')
            this.checked = true
        }
        // this.canvas()
        this.getLoginUrl();
        this.getPicture();
        this.$nextTick(() => {
            setTimeout(() => {
                $('.meta').attr('content', 'width=device-width, initial-scale=1, maximum-scale=10, user-scalable=yes')
            }, 10)

        })

    },
    watch: {
        $route: {
            handler: function (route) {
                this.redirect = route.query && route.query.redirect
            },
            immediate: true
        }
    },
    methods: {
        // canvas() {
        //   var WIDTH = window.innerWidth, HEIGHT = window.innerHeight, POINT = 35;

        //   var canvas = document.getElementById('Mycanvas');
        //   canvas.width = WIDTH,
        //   canvas.height = HEIGHT;
        //   var context = canvas.getContext('2d');
        //   context.strokeStyle = 'rgba(0,0,0,0.2)',
        //   context.strokeWidth = 1,
        //   context.fillStyle = 'rgba(0,0,0,0.1)';
        //   var circleArr = [];

        //   //线条：开始xy坐标，结束xy坐标，线条透明度
        //   function Line (x, y, _x, _y, o) {
        //       this.beginX = x,
        //       this.beginY = y,
        //       this.closeX = _x,
        //       this.closeY = _y,
        //       this.o = o;
        //   }
        //   //点：圆心xy坐标，半径，每帧移动xy的距离
        //   function Circle (x, y, r, moveX, moveY) {
        //       this.x = x,
        //       this.y = y,
        //       this.r = r,
        //       this.moveX = moveX,
        //       this.moveY = moveY;
        //   }
        //   //生成max和min之间的随机数
        //   function num (max, _min) {
        //       var min = arguments[1] || 0;
        //       return Math.floor(Math.random()*(max-min+1)+min);
        //   }
        //   // 绘制原点
        //   function drawCricle (cxt, x, y, r, moveX, moveY) {
        //       var circle = new Circle(x, y, r, moveX, moveY)
        //       cxt.beginPath()
        //       cxt.arc(circle.x, circle.y, circle.r, 0, 2*Math.PI)
        //       cxt.closePath()
        //       cxt.fill();
        //       return circle;
        //   }
        //   //绘制线条
        //   function drawLine (cxt, x, y, _x, _y, o) {
        //       var line = new Line(x, y, _x, _y, o)
        //       cxt.beginPath()
        //       cxt.strokeStyle = 'rgba(0,0,0,'+ o +')'
        //       cxt.moveTo(line.beginX, line.beginY)
        //       cxt.lineTo(line.closeX, line.closeY)
        //       cxt.closePath()
        //       cxt.stroke();

        //   }
        //   //初始化生成原点
        //   function init () {
        //       circleArr = [];
        //       for (var i = 0; i < POINT; i++) {
        //           circleArr.push(drawCricle(context, num(WIDTH), num(HEIGHT), num(15, 2), num(10, -10)/40, num(10, -10)/40));
        //       }
        //       draw();
        //   }

        //   //每帧绘制
        //   function draw () {
        //       context.clearRect(0,0,canvas.width, canvas.height);
        //       for (var i = 0; i < POINT; i++) {
        //           drawCricle(context, circleArr[i].x, circleArr[i].y, circleArr[i].r);
        //       }
        //       for (var i = 0; i < POINT; i++) {
        //           for (var j = 0; j < POINT; j++) {
        //               if (i + j < POINT) {
        //                   var A = Math.abs(circleArr[i+j].x - circleArr[i].x),
        //                       B = Math.abs(circleArr[i+j].y - circleArr[i].y);
        //                   var lineLength = Math.sqrt(A*A + B*B);
        //                   var C = 1/lineLength*7-0.009;
        //                   var lineOpacity = C > 0.03 ? 0.03 : C;
        //                   if (lineOpacity > 0) {
        //                       drawLine(context, circleArr[i].x, circleArr[i].y, circleArr[i+j].x, circleArr[i+j].y, lineOpacity);
        //                   }
        //               }
        //           }
        //       }
        //   }

        //   //调用执行
        //   window.onload = function () {
        //       init();
        //       setInterval(function () {
        //           for (var i = 0; i < POINT; i++) {
        //               var cir = circleArr[i];
        //               cir.x += cir.moveX;
        //               cir.y += cir.moveY;
        //               if (cir.x > WIDTH) cir.x = 0;
        //               else if (cir.x < 0) cir.x = WIDTH;
        //               if (cir.y > HEIGHT) cir.y = 0;
        //               else if (cir.y < 0) cir.y = HEIGHT;

        //           }
        //           draw();
        //       }, 16);
        //   }
        // },
        showPwd() {
            if (this.passwordType === 'password') {
                this.passwordType = ''
            } else {
                this.passwordType = 'password'
            }
            this.$nextTick(() => {
                this.$refs.password.focus()
            })
        },
        // 判断是否记住密码
        isSave () {
            console.log(this.checked)
            if (this.checked) {
                
            }
        },

        //判断账号是否有权限
        // getUserTree() {
        //     const routes = router.options.routes
        //     const newRoute = []
        //     routes.forEach(e=>{
        //         newRoute.push({...e})
        //     })
        //     asyncRoutes.forEach(e=>{
        //         newRoute.push({...e})
        //     })
        //     router.options.routes = newRoute
        //     router.addRoutes(newRoute)
        //     console.log('newRoute', newRoute)
        //     console.log('asyc', asyncRoutes)
        //     console.log(router)
        //     getDatas({}, '/user/function/getUserTree').then(res => {
        //         if (res.code == 1001) {
        //             if (res.body.length > 0) {
        //                 this.$router.push({
        //                     path: this.redirect || '/'
        //                 })

        //                 $('.meta').attr('content', 'width=device-width, initial-scale=0.1, maximum-scale=10, user-scalable=yes')
        //             } else {
        //                 this.$message({
        //                     showClose: true,
        //                     message: '您无权限登录，请联系管理员',
        //                     type: 'error',
        //                 });
        //             }
        //         } else {
        //             this.$message({
        //                 showClose: true,
        //                 message: res.msg,
        //                 type: 'error',
        //             });
        //         }
        //     }).catch(err => {

        //     })
        // },
        handleLogin() {
            sessionStorage.clear();

            this.$refs.loginForm.validate(valid => {
                //console.log(this.loginForm)
                if (valid) {
                    if (this.checked) {
                        localStorage.setItem('user', this.loginForm.username)
                        localStorage.setItem('secret', this.loginForm.password)
                    } else {
                        localStorage.removeItem('user')
                        localStorage.removeItem('secret')
                    }
                    this.loading = true
                    this.$store.dispatch('user/login', this.loginForm).then((res) => {
                        if (res.code == '1001') {
                            sessionStorage.setItem("userType", res.body.userType)
                            // this.getUserTree() //判断账户是否有权限
                            //this.$router.push({ path: this.redirect || '/' })
                            this.$router.push('/')
                            //$('.meta').attr('content','width=device-width, initial-scale=0.1, maximum-scale=10, user-scalable=yes')
                        } else {
                            this.$message({
                                message: res.msg,
                                type: 'error',
                                duration: 5 * 1000
                            })
                        }
                        this.loading = false
                    }).catch(() => {
                        this.loading = false
                    })
                } else {
                    console.log('error submit!!')
                    return false
                }
            })
        },
        getLoginUrl() {
            getDatas({userType: 3}, '/user/qqConfig/getLoginUrl').then(res => {
                if (res.code === 1001) {
                    this.pageData.qq = res.body
                }
                this.pageData.wx = ''
            })
        },
        getPicture() {
            // this.GET(
            //     '/platform/platformBase/getBySiteDomain', {},
            //     data => {
            //         if (data.code === 1001 && data.body) {
            //             this.loginLogo = data.body
            //         }
            //     }
            // )
            getDatas({}, '/platform/platformBase/getByDomain').then(res => {
                if (res.code === 1001) {
                    this.loginLogo = res.body.logo
                } else {
                    this.loginLogo = 'http://zong.111ks.cn/Public/logo.png'
                }
            })
        }
    }
}
</script>

<style lang="scss">
/* 修复input 背景不协调 和光标变色 */
/* Detail see https://github.com/PanJiaChen/vue-element-admin/pull/927 */

$bg:#f0f2f5;
$light_gray:#fff;
$cursor: #fff;

@supports (-webkit-mask: none) and (not (cater-color: $cursor)) {
    .login-container .el-input input {
        // color: $cursor;
        // color:red;
    }
}

.login-container .el-input input {
    // color:red;
}

#Mycanvas {
    position: absolute;
    width: 100%;
    height: 100%;
}

/* reset element-ui css */
.login-container {
    // .el-input {
    //     display: inline-block;
    //     height: 47px;
    //     width: 85% !important;

    //     input {
    //         background: transparent;
    //         border: 0px;
    //         -webkit-appearance: none;
    //         border-radius: 0px;
    //         padding: 12px 5px 12px 15px;
    //         // color: $light_gray;
    //         height: 47px;
    //         // caret-color: $cursor;

    //         &:-webkit-autofill {
    //             // box-shadow: 0 0 0px 1000px $bg inset !important;
    //             // -webkit-text-fill-color: $cursor !important;
    //         }
    //     }
    // }

    // .el-form-item {
    //     border: 1px solid rgba(255, 255, 255, 0.1);
    //     // background: rgba(0, 0, 0, 0.1);
    //     border: 1px solid #bbb;
    //     border-radius: 5px;
    //     color: #454545;
    // }
}
</style><style lang="scss" scoped>
$bg:#f0f2f5;
$dark_gray:#889aa4;
$light_gray:#eee;

.login-container {
    min-height: 100%;
    width: 100%;
    background-color: $bg;
    overflow: hidden;

    .login-form {
        position: absolute;
        width: 320px;
        max-width: 100%;
        padding: 40px 30px;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background: white;

        & .third {
            // margin-top: 20px;
            text-align: center;

            &>a>span {
                width: 48px;
                height: 48px;
                display: inline-block;
                background: center center no-repeat;
                background-size: 100% auto;
            }

            .qq {
                background-image: url('~@/assets/login_qq.png');
                margin-right: 50px;
            }

            .wx {
                background-image: url('~@/assets/login_wx.png');
            }
        }
    }

    .tips {
        font-size: 14px;
        color: #fff;
        margin-bottom: 10px;

        span {
            &:first-of-type {
                margin-right: 16px;
            }
        }
    }

    .svg-container {
        padding: 6px 5px 6px 15px;
        color: $dark_gray;
        vertical-align: middle;
        width: 30px;
        display: inline-block;
    }

    .title-container {
        text-align: center;
        margin-bottom: 22px;

        & img {
            width: 200px
        }
    }

    .show-pwd {
        position: absolute;
        right: 10px;
        top: 7px;
        font-size: 16px;
        color: $dark_gray;
        cursor: pointer;
        user-select: none;
    }
}
</style>
