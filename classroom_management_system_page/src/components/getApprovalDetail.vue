<template>
<div>
  <header>
    <div id="title">审批</div>
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
  <div>

      <div class="crumbs">
        <div class="cantainer">
          <el-table :data="tableData" style="width: 100%">
            <el-table-column
              prop="teachingBuilding"
              label="教学楼"
              width="70">
            </el-table-column>
            <el-table-column
              prop="roomNumber"
              label="教室号"
              width="70">
            </el-table-column>
            <el-table-column
              prop="date"
              label="日期"
              width="70">
            </el-table-column>
            <el-table-column
              prop="time"
              label="时间"
              width="70">
            </el-table-column>
            <el-table-column
              prop="userName"
              label="申请人"
              width="70">
            </el-table-column>
            <el-table-column
              prop="reservationDesc"
              label="申请描述"
              width="80">
            </el-table-column>
            <el-table-column
              prop="address"
              label="操作"
              width="80">
              <el-row style="width: 100%;margin: 0;padding: 0">
                <el-button size="mini">同意</el-button>
                <el-button size="mini">驳回</el-button>
              </el-row>
            </el-table-column>
            <el-table-column
              prop="address"
              label="驳回原因">
              <span @click="open3">理由</span>
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
</div>
</template>

<script>
  import axios from 'axios'
  import { MessageBox } from 'mint-ui'
  export default {
    data() {
      return {
        tableData:[],
        date:'',
        reservationDesc:'',
        roomNumber:'',
        teachingBuilding:'',
        userName:'',
        time:'',
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
      }},
    created () {
      let userId = this.$route.query.userId
      let id = this.$route.query.id
      this.userId = userId
      this.id = id
      console.log(this.userId)
      console.log(this.id)
      this.getmsg()
    },
    mounted(){
      /*如果cookie不存在，则跳转到登录页*/
      if(uname ===""){
        this.$router.push('/')
      }
    },
    methods: {
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
      handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
      },
      handleCurrentChange(val) {
        console.log(`当前页: ${val}`);
      },
      tohome:function () {
        this.$router.push({path:'./#'})
      },
      getmsg:function(){
        let self = this
        let data1 = ({

        })
        console.log(data1)
        axios({
          method: 'get',
          url: 'http://yizhuoyang.free.idcfengye.com/rsv/high/getApprovalDetail',
          data: data1
        }).then((response) => {
          console.log(response)
          let tableData=[]
          console.log(eval(response.data.data))
          this.tableData = eval(response.data.data)
          console.log(self.tableData)

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
