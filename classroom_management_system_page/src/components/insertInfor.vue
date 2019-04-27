<template>
  <div>
    <header>
      <div id="title" >教室预约</div>
      <i class="el-icon-arrow-left" @click="tohome"></i>
    </header>
    <p>教室详情</p>
    <el-row :gutter="16">
      <el-col :span="8">教学楼号：</el-col>
      <el-col :span="6">{{teachingBuilding}}</el-col>
    </el-row>
    <el-row :gutter="16">
      <el-col :span="8">教室号：</el-col>
      <el-col :span="6">{{roomNumber}}</el-col>
    </el-row>
    <el-row :gutter="16">
      <el-col :span="8">教室位置：</el-col>
      <el-col :span="6">{{roomLocal}}</el-col>
    </el-row>
    <el-row :gutter="16">
      <el-col :span="8">座位数：</el-col>
      <el-col :span="6">{{seatsNumber}}</el-col>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="8">教室器材：</el-col>
      <el-col :span="8">{{multimediaEquipment}}</el-col>
    </el-row><br>
    <span>请选择日期</span>
    <div>
      <!--<div class="block">
        <mt-datetime-picker
          v-model="pickerValue"
          type="date"
          year-format="{value} 年"
          month-format="{value} 月"
          date-format="{value} 日"
          @confirm="handleConfirm()"
        >开始
        </mt-datetime-picker>
        <mt-datetime-picker
          v-model="pickerValue"
          ref="picker"
          type="time"
          year-format="{value} 年"
          month-format="{value} 月"
          date-format="{value} 日"
        >
        </mt-datetime-picker>
      </div>-->
      <div>
        <el-table :data="tableData" style="width: 100%">
          <el-table-column
            prop="time"
            label="时间"
            width="80">
          </el-table-column>
          <el-table-column
            prop="status"
            label="状态"
            width="100">
          </el-table-column>
          <el-table-column
            prop="address"
            label="申请描述">
            <span @click="open3">理由</span>
          </el-table-column>
          <el-table-column
            prop="address"
            label="操作">
            <span @click="toinserfor">预约</span>
          </el-table-column>
        </el-table>
      </div>
    </div>

  </div>
</template>

<script>
  import 'mint-ui/lib/style.min.css'
  import axios from 'axios'
  import { Toast } from 'mint-ui'
  import { MessageBox } from 'mint-ui'
  import { setCookie,getCookie,delCookie } from '../assets/cookie.js'
  import { DatetimePicker } from 'mint-ui'
  /*Vue.component(DatetimePicker.name, DatetimePicker)*/
  import Bus from '../assets/Bus.js'
    export default {
        name: "insertInfor",
      data(){
        return {
          tableData:[],
          userId: this.userId,
          teachingBuilding:'',
          roomNumber:'',
          seatsNumber:'',
          roomLocal:'',
          id:this.id,
          status:'',
          st:'',
          time:'',
          textarea: '',
          multimediaEquipment:'',
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
          value7: '',
          date:''
        }
      },
      created() {
        axios.get('http://localhost:80/cls/getRoomDetailByIdAndDate', {
          params: {
            'id': '1',
            "date": "20190423"
          }
        }).then((response) => {
          console.log(response.data.data)
          let tableData=[]
          let data =eval(response.data.data)
          let roomdata = data.classroom
          self.tableData = eval(data.status)
          console.log(tableData)
          self.seatsNumber= roomdata.seatsNumber
          self.roomLocal=roomdata.roomLocal
          self.roomNumber = roomdata.roomNumber
          self.teachingBuilding=roomdata.teachingBuilding
          self.multimediaEquipment=roomdata.multimediaEquipment
        })
          .catch(function (err) {
            console.log(err);
          })
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
        selectData () { // 打开时间选择器

          if (this.selectedValue) {
            this.dateVal = this.selectedValue
          } else {
            this.dateVal = new Date()
          }
          this.$refs['datePicker'].open()
        },
        dateConfirm () { // 时间选择器确定按钮，并把时间转换成我们需要的时间格式
          this.selectedValue = formatDateMin(this.dateVal)
      },
        openPicker() {
          this.$refs.picker.open();
        },
        open3() {
          MessageBox.prompt('请输入理由').then(({ value, action }) => {
            this.$message({
              type: 'success',
              message: '理由: ' + value
            });
          }).catch(() => {
            this.$message({
              type: 'info',
              message: '取消输入'
            });
          });
      },
        open1() {
          Toast({
            message: '预约成功',
            iconClass: 'icon icon-success'
          });
        },
        open2() {
        Toast({
        message: '预约失败',
        iconClass: 'icon icon-success'
        });

        },
        toinserfor:function(){
          let self = this
          axios.post( 'http://yizhuoyang.free.idcfengye.com/rsv/insertInfo', {
            params: {
              "roomId": this.$route.query.id,
              "date": "20190306",
              "time": "3",
              "userId": this.userId,
              "userName": "zhangsan",
              "reservationDesc": "计算机协会又来了"
            }
          })
            .then(function (response) {
              console.log(response.data.message)
              if(response.data.message==='success'){
                self.open1()
              }else{
                self.open2()
              }
            })
            .catch(function (err) {
              console.log(err);
            })
        },
        tohome:function () {
          this.$router.push({path:'./'})
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
  .block{
    height: 2rem;
    width: 100%;
    background-color: #ccc;
  }
</style>
