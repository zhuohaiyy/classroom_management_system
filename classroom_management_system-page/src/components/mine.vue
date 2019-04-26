<template>
<div>
  <header>
    <div id="title">我的预约</div>
    <i class="el-icon-arrow-left" @click="tohome"></i>
  </header>

  <div>
    <el-date-picker
      v-model="value7"
      size="mini"
      type="daterange"
      align="right"
      unlink-panels
      range-separator="至"
      start-placeholder="开始日期"
      end-placeholder="结束日期"
      :picker-options="pickerOptions2">
    </el-date-picker>
  </div>
  <div class="crumbs">
    <div class="cantainer">
      <el-table :data="tableData" style="width: 100%">
        <el-table-column
          prop="teachingBuilding"
          label="教学楼"
          width="80">
        </el-table-column>
        <el-table-column
          prop="roomNumber"
          label="教室号"
          width="80">
        </el-table-column>
        <el-table-column
          prop="date"
          label="日期"
          width="80">
        </el-table-column>
        <el-table-column
          prop="time"
          label="时间"
          width="80">
        </el-table-column>
      </el-table>
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page.sync="currentPage2"
        :page-sizes="[10, 20, 30, 40]"
        :page-size="10"
        layout="sizes, prev, pager, next"
        :total="50">
      </el-pagination>
    </div>
  </div>
</div>
</template>

<script>
  import axios from 'axios'
  import { setCookie,getCookie,delCookie } from '../assets/cookie.js'
    export default {
        name: "mine",
      data(){
          return{
            date:'',
            userId:'',
            roomId:'',
            roomNumber:'',
            teachingBuilding:'',
            time:'',
            user:'点击登录',
            tableData:[],
            pickerOptions2: {
              shortcuts: [{
                text: '最近一周',
                onClick(picker) {
                  const end = new Date();
                  const start = new Date();
                  start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
                  picker.$emit('pick', [start, end]);
                }
              }]
            },
            value6: '',
            value7: ''
          }
      },
      created () {

      },
      mounted(){
        let uname = getCookie('userId')
        console.log(uname)
        this.userId = uname
        console.log(this.userId)
        /*如果cookie不存在，则跳转到登录页*/
        if(uname ===""){
          this.$router.push('/')
        }
        this.getmsge()
      },
      methods:{
        getmsge:function(){
          let self = this
          axios.get( 'http://yizhuoyang.free.idcfengye.com//rsv/getStudentRSVById', {
            params: {
              'uid':this.userId
            }
          })
            .then(function (response) {
              console.log(response)
              let tableData=[]
              self.tableData = eval(response.data.data)
              console.log(self.tableData)
            })
            .catch(function (err) {
              console.log(err);
            })
        },
        tohome:function () {
          this.$router.push({path:'./#',query:{'userId': "this.userId"}})
        },
        login:function () {
          this.$router.push({path:'./login'})
        },
        handleSizeChange(val) {
          console.log(`每页 ${val} 条`);
        },
        handleCurrentChange(val) {
          console.log(`当前页: ${val}`);
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
  .el-icon-circle-close{
    font-size: 1.15rem;
    position: absolute;
    right: 2.5rem;
    top: 5.65rem;
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
  .login{
    text-align: left;
    margin-left: 50px;
  }
</style>
