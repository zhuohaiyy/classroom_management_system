<template>
<div>
  <header>
    <div id="title">教室查询</div>
    <i class="el-icon-arrow-left" @click="tohome"></i>
  </header>
  <div>
    <el-row>
      <el-col :span="12"> <span class="demonstration">教学楼:</span>
        <el-select v-model="value1" placeholder="请选择">
          <el-option
            v-for="item in options"
            :key="item.value1"
            :label="item.label"
            :value="item.value1">
          </el-option>
        </el-select></el-col>
      <el-col :span="12">
        <span class="demonstration">教室大小:</span>
        <el-select v-model="value2" placeholder="请选择">
          <el-option
            v-for="item in roomsize"
            :key="item.value2"
            :label="item.label"
            :value="item.value2">
          </el-option>
        </el-select>
      </el-col>
    </el-row>

  </div><br>
  <div>
    <el-row>
      <el-col :span="8"><span>教室查询:</span></el-col>
      <el-col :span="14"><el-input placeholder="请输入内容" v-model="input5" >
        <el-button slot="append" icon="el-icon-search" @click="room"></el-button>
      </el-input></el-col>
    </el-row>

  </div>
  <div>
    <el-table
      :data="tableData"
      style="width: 100%" @row-click="openDetails">
      <el-table-column
        prop=teachingBuilding
        label="教学楼"
        width="65" >
      </el-table-column>
      <el-table-column
        prop="roomNumber"
        label="教室号"
        width="65">
        <template slot-scope="scope" style="display: none">
          {{scope.$index+1}}
        </template>
      </el-table-column>
      <el-table-column
        prop="seatsNumber"
        label="座位大小"  width="80">
      </el-table-column>
      <el-table-column
        prop="roomLocal"
        label="教室地址" width="80" >
      </el-table-column>
      <el-table-column prop="id" label="操作" width="80" @blur="reid">
        <template slot-scope="{row}">
          <div>
            <span @click="insertInfor(row.id)">详情</span>
          </div>
        </template>
      </el-table-column>
    </el-table>
  </div>
</div>
</template>

<script>
  import axios from 'axios'
  import Bus from '../assets/Bus.js'
    export default {
        name: "getRoom",
      data(){
          return{
            teachingBuilding:'',
            roomNumber:'',
            seatsNumber:'',
            roomLocal:'',
            input5:'',
            id:'',
            tableData:this.tableData,
            options: [{
              value1: '选项1',
              label: '教一'
            }, {
              value1: '选项2',
              label: '教二'
            }],
            roomsize:[{
              value2:'选项一',
              label:'大'
            },{
              value2:'选项二',
              label:'小'
            }],
            value1: '',
            value2:''
          }
      },
      mounted:function(){

        let userId = this.$route.query.userId
        this.userId = userId
        console.log(this.userId)

        let self = this
        let data1 = ({
        })
        console.log(data1)
        axios({
          method: 'get',
          url: 'http://yizhuoyang.free.idcfengye.com/cls/getRoomList',
          data: data1
        }).then((response) => {
          let tableData = []

          self.tableData = response.data.data.parse()

        }).catch(error => function () {
        })
      },
      methods:{
        openDetails(row, event, column){
          let reid=row.id
          this.id=reid
          console.log(this.id)
          console.log(row.id)
        },
        tohome:function () {
          this.$router.push({path:'./#',query:{'userId': this.userId}})
        },
        insertInfor: function (id) {
          this.$router.push({
            path: './insertInfor',
            query: {
              id
            }
          })
        },
        room : function(){
          let self = this
          let data1 = ({
            "teachingBuilding": this.options,
            "size": this.roomsize,
            "room_number":this.input5
          })
          console.log(data1)
          axios({
            method: 'get',
            url: 'http://yizhuoyang.free.idcfengye.com/cls/getRoomByCondition',
            data: data1
          }).then((response) => {
            let tableData=[]
            console.log(eval(response.data.data))

           self.tableData = eval(response.data.data)
            console.log(self.tableData)
           console.log(self.tableData[i].id)

          }).catch(error => function () {
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
</style>
