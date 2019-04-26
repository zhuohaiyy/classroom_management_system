<template>
  <div>
    <header>
      <div id="title" @click="getmsge">教室预约</div>
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
      <div class="block">
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
  import axios from 'axios'
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
          id:'',
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

      created () {
        let userId = this.$route.query.userId
        let id = this.$route.query.id
        this.userId = userId
        this.id = id
        console.log(this.userId)
        console.log(this.id)
        this.getmsge()
      },
      methods:{
        open3() {
          this.$prompt('请输入理由', '提示', {
            confirmButtonText: '确定',

          }).then(({ value }) => {
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
          this.$alert('预约成功', {
            confirmButtonText: '确定',
            callback: action => {
              this.$message({
                type: 'info',
                roundButton:ture,
                message: `action: ${ action }`
              });
            }
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
              "roomId": "1",
              "date": "20190306",
              "time": "3",
              "userId": "10000111",
              "userName": "zhangsan",
              "reservationDesc": "计算机协会又来了"
            }
          })
            .then(function (response) {
              console.log(response.data.message)
              if(response.data.message==='success'){
                  self.open1()
              }
              else if(response.data.message==='fail'){
               self.open2()
              }
            })
            .catch(function (err) {
              console.log(err);
            })
        },
        tohome:function () {
          this.$router.push({path:'./#',query:{'userId': this.userId}})
        },
        getmsge:function(){
          let self = this
          axios.get( 'http://yizhuoyang.free.idcfengye.com/cls/getRoomDetailByIdAndDate', {
            params: {
              'id': '1',
              'date':'20190413'
        }
        })
        .then(function (response) {
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
</style>
