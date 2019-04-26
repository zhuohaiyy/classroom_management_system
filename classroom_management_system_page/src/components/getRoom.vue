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
      style="width: 100%">
      <el-table-column
        prop=teachingBuilding
        label="教学楼"
        width="65" >
      </el-table-column>
      <el-table-column
        prop="roomNumber"
        label="教室号"
        width="65">
      </el-table-column>
      <el-table-column
        prop="seatsNumber"
        label="座位大小"  width="80">
      </el-table-column>
      <el-table-column
        prop="roomLocal"
        label="教室地址" width="80">
      </el-table-column>
      <el-table-column
        prop="id"
        label="操作" width="80" >
        <span @click="insertInfor">详情</span>
      </el-table-column>
    </el-table>
  </div>
</div>
</template>

<script>
  import axios from 'axios'
    export default {
        name: "getRoom",
      data(){
          return{
            value1: '',
            value2:'',
            teachingBuilding:'',
            roomNumber:'',
            seatsNumber:'',
            roomLocal:'',
            input5:'',
            id:'',
            tableData:[],
            options: [{
              value1: '教1',
              label: '教一'
            }, {
              value1: '教2',
              label: '教二'
            }],
            roomsize:[{
              value2:'1',
              label:'大'
            },{
              value2:'2',
              label:'小'
            }]
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
        tohome:function () {
          this.$router.push({path:'./#',query:{'userId': this.userId}})
        },
        insertInfor:function(){
          this.$router.push({path:'./insertInfor',query:{'userId': this.userId,'id': this.id}})
          console.log(id)
        },
room : function(){
  let self = this
  axios.get( 'http://yizhuoyang.free.idcfengye.com/cls/getRoomByCondition', {
    params: {
      "teachingBuilding": this.value1,
      "size": this.value2 ,
      "room_number":this.input5
      }
  })
    .then((response) => {
    let tableData=[]
    console.log(eval(response.data.data))

    self.tableData = eval(response.data.data)
    console.log(self.tableData)
    })
    .catch(function (err) {
      console.log(err);
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
