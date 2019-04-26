<template>
<div class="a">
  <header>
    <div id="title">首页</div>
  </header>
  <a><div class="tologin" @click="login">{{userId}}</div>
    <div class="quit" @click="quit">退出</div>
  </a>
  <img class="welcome" src="../assets/welcome.jpg"/>
  <div class="footer">
    <el-row>
      <el-button type="primary" plain style="margin:1rem" @click="getRoom">教室查询</el-button>
      <el-button type="success" plain style="margin:1rem" @click="insertInfor">教室预约</el-button><br>
      <el-button type="info" plain style="margin:1rem" @click="getRoomDetailByIdAndDate">教室审批</el-button>
      <el-button type="warning" plain style="margin:1rem" @click="tomine">我的预约</el-button>

    </el-row>
  </div>
</div>
</template>

<script>
  import foowrap from "./foowrap"
  import { setCookie,getCookie,delCookie } from '../assets/cookie.js'
  import axios from "axios"
    export default {
        name: "home",
      data(){
         return{
           ol:'登录注册',
           username:'',
           userId:this.userId,
           foowrap:foowrap
         }
      },
      components: {
        foowrap: foowrap
      },
      mounted(){
        /*页面挂载获取保存的cookie值，渲染到页面上*/
        let uname = getCookie('userId')
        console.log(uname)
        this.userId = uname
        /*如果cookie不存在，则跳转到登录userId页*/
        if(uname === ""){
          this.userId = '登录/注册'
        }
      },
      methods:{
        quit(){
          /*删除cookie*/
          delCookie('userId')
          location. reload()
          this.$router.go(0)
        },
        login:function () {
          this.$router.push({path:'./login'})
        },
        tomine:function(){
          this.$router.push({path:'./mine',query:{'userId': this.userId,'username':this.username}})
        },
        getRoom:function () {
          this.$router.push({path:'./getRoom',query:{'userId': this.userId}})
        },
        insertInfor:function(){
          this.$router.push({path:'./insertInfor',query:{'userId': this.userId}})
        },
        getRoomDetailByIdAndDate:function () {
          this.$router.push({path:'./getApprovalDetail',query:{'userId': this.userId}})
        },
        gin:function(){
          let data1 = ({
            "userId": this.userId,
            "password": this.password,
            "rememberMe":this.rememberMe
          })
          console.log(data1)
          axios({
            method: 'get',
            url: 'http://yizhuoyang.free.idcfengye.com/login/logout',
            data: data1
          }).then((response) => {
            console.log(response.data.code)
            if (response.data.code === 1) {
              this.$router.push({path: '/#', query: {'uid': this.uId}})
            } else if (response.data === 0) {
            }
          }).catch(error => function () {
          })
        }
      }
    }
</script>

<style scoped>
  header #title{
    float: left;
    text-align: center;
    font-size: 1.2rem;
    width: 100%;
    height: 1.58rem;
    line-height: 1.68rem;
    margin-bottom: 1rem;
  }
  .a{
    width: 100%;
    height: 600px;
    background-image: url("../assets/home.jpg");
    background-size: 100% 600px;
    position: relative;
  }
  .tologin{
    text-align: right;
    margin-top: 10px;
    margin-left: 13rem;
    float: left;
  }
  .quit{
    text-align: right;
    margin-top: 10px;
    margin-right: 20px;
    float: right;
  }
.welcome{
  width: 90%;
  margin: 85px 5% 0 5%;
}
  .footer{

    width: 100%;
    position: absolute;
    bottom: 60px;
  }
</style>
