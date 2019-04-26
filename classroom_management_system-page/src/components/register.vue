<template>
  <div>
    <header>
      <i class="el-icon-arrow-left" @click="tologin"></i>
      <div id="title" @click="register">注册</div>
    </header>

    <div class="pass">
      <el-row>
        <el-col :span="6">学号：</el-col>
        <el-col :span="14"><el-input
          placeholder="请输入学号"
          v-model="userId">
        </el-input></el-col>
      </el-row>
    </div>
    <div class="pass">
      <el-row>
        <el-col :span="6">姓名：</el-col>
        <el-col :span="14"><el-input
          placeholder="请输入姓名"
          v-model="username">
        </el-input></el-col>
      </el-row>
    </div>

    <div class="pass">
      <el-row>
        <el-col :span="6">密码：</el-col>
        <el-col :span="14"><el-input
          placeholder="请输入密码"
          v-model="password" type="password">
        </el-input></el-col>
      </el-row>
    </div>

    <div class="pass">

      <el-row>
        <el-col :span="6"> 性别：</el-col>
        <el-col :span="18" style="margin-left: -25px;"><el-radio v-model="sex" label="1" border>&nbsp;&nbsp;男&nbsp;&nbsp;</el-radio>
          <el-radio v-model="sex" label="2" border>&nbsp;&nbsp;&nbsp;女&nbsp;&nbsp;</el-radio></el-col>
      </el-row>
    </div>

    <div class="pass">
      <el-row>
        <el-col :span="6"> 身份：</el-col>
        <el-col :span="18" style="margin-left: -22px;"> <el-radio v-model="roles" label="1" border >学&nbsp;生</el-radio>
          <el-radio v-model="roles" label="2" border>管理员</el-radio></el-col>
      </el-row>

    </div>
    <div class="btn">
      <el-button type="primary" @click="register">   注册   </el-button>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'
    export default {
        name: "register",
      data(){
        return{
          userId:'',
          username:'',
          password:'',
          sex:true,
          roles:'',
          perms:" user:add"
        }
      },
      methods:{
        tologin : function () {
          this.$router.push({path:'./login'})
        },
        register:function(){
          let data1 = ({
            "userId":this.userId,
            "username":this.username,
            "password":this.password,
            "sex":true,
            "roles":'student',
            "perms":" user:add"
          })
          axios({
            method: 'post',
            url: 'http://yizhuoyang.free.idcfengye.com/login/register',
            data: data1,

            headers: {'Content-Type': 'application/json'}

          }).then((response) => {
            this.uId = response.data
            if (response.data !== 0) {
              {
                this.$router.push({path:'./login'})
              }
            } else if (response.data === 0) {
              this.telNum = null
              this.pw = null
            }
            console.log(this.uId)
          }).catch(error => function () {
            console.log(error)
          })
        }
      }
    }
</script>

<style scoped>
  header{
    width: 100%;
    height:1.58rem;
    background-color: #fff;
  }
 .el-icon-arrow-left{
    font-size: 1.45rem;
    position: absolute;
    left: 1.5rem;
    top: 3.65rem;
  }
  header #title{
    float: left;
    text-align: center;
    font-size: 1.2rem;
    width: 100%;
    height: 1.58rem;
    line-height: 1.68rem;
    margin-bottom: 1rem;
  }
  .pass{
    margin-bottom: 1rem;
  }
</style>
