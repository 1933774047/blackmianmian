<template>
  <div class='container' style="overflow: hidden; padding-left: 20px ; padding-top: 20px">
    <el-row :gutter="20">
      <el-col :span="12"><div class="grid-content bg-purple">
        <el-form>
          <el-form-item label="关键字" style="padding-left: 20px ;" >
            <el-input  placeholder="根据编号搜索" style="width: 50%;" v-model="inputData"></el-input>
          </el-form-item>
        </el-form>
      </div></el-col>
      <el-col :span="12"><div class="grid-content bg-purple">
        <el-row  style="padding-left: 480px ;">
          <el-button @click="deleteInput()">清除</el-button>
          <el-button type="primary" @click="searchInput(inputData)">搜索</el-button>
        </el-row>
      </div></el-col>
    </el-row>
    <el-row>
      <el-alert
        type="info"
        show-icon>
        数据一共{{ total }}条
      </el-alert>
    </el-row>
    <el-row>
    <el-table :data="tableData" border style="width: 100%">
      <el-table-column fixed prop="id" label="编号" width="250">
      </el-table-column>
      <el-table-column prop="questionType" label="题型" width="100">
      </el-table-column>
      <el-table-column prop="questionIDs" label="题目编号" width="200" >
        <template slot-scope="scope">
          <div v-html="item.number" v-for="item in scope.row.questionIDs" style="color: rgb(0, 153, 255)"></div>
        </template>
      </el-table-column>
      <el-table-column prop="addTime" label="录入时间" width="180">
      </el-table-column>
      <el-table-column prop="totalSeconds" label="答题时间(s)" width="180">
      </el-table-column>
      <el-table-column prop="accuracyRate" label="正确率(%)" width="180"> </el-table-column>
      <el-table-column prop="userName" label="录入人" width="100"> </el-table-column>
      <el-table-column
        fixed="right"
        label="操作"
        width="100">
        <template v-slot="{row}">
          <el-button type="danger" icon="el-icon-delete" circle plain @click="deleteList(row)"></el-button>
        </template>
      </el-table-column>
    </el-table>
  </el-row>
    <el-row class="block" style="padding-left:700px">
      <el-pagination
        layout="prev,pager,next,sizes,jumper"
        :page-sizes="[20,30, 40,50]"
        :page-size="pageParams.pagesize"
        :total="total"
        :current-page.sync="curPage"
        @current-change="hCurrentChange"
        @size-change="hSizeChange"
      />
    </el-row>
    </div>
</template>

<script>
import { randoms, removeRandoms } from '@/api/hmmm/questions'
export default {
  data () {
    return {
      // input 的值
      inputData: '',
      pageParams: {
        // 页码  每页几条
        page: 1,
        pagesize: 20
      },
      curPage: 0, // 当前页数
      list1: [], // 角色列表
      total: 0, // 一共几条数据
      tableData: [
        {
          id: '',
          questionType: '',
          questionIDs: '',
          addTime: '',
          totalSeconds: '',
          accuracyRate: '',
          userName: ''
        }
      ]
    }
  },
  created () {
    this.randomsList()
  },
  methods: {
    // 删除数据
    async  deleteList (id) {
      console.log(id.id)
      await this.$confirm('确认删除!')
      await removeRandoms({
        id: id.id
      })
      this.$message.success('删除成功!')
      await this.randomsList()
    },
    // 搜索
    async searchInput (value) {
      console.log(value)
      const res = await randoms({
        page: this.pageParams.page,
        pagesize: this.pageParams.pagesize,
        keyword: value
      })
      // console.log(res.data.items)
      this.total = res.data.counts
      this.tableData = res.data.items
    },
    // pageSize 改变时会触发
    // 每页几条
    // 用户调整了每页显示的条数
    hSizeChange (pagesize) {
      // 1. 更新每页条数
      this.pageParams.pagesize = pagesize
      // 2. 重发请求
      this.randomsList()
    },
    hCurrentChange (curPage) {
      // alert(curPage)
      // 1. 更新页码
      this.pageParams.page = curPage
      // 2. 重发请求
      this.randomsList()
    },
    //  获取组题列表
    async randomsList () {
      const res = await randoms({
        page: this.pageParams.page,
        pagesize: this.pageParams.pagesize
      })
      // console.log(res.data.items)
      this.total = res.data.counts
      this.tableData = res.data.items
    },
    // 清空 input
    deleteInput () {
      // console.log(this.inputData)
      this.inputData = ''
    }
  }
}
</script>

<style scoped lang='less'>
.el-row {
  margin-bottom: 20px;
  &:last-child {
    margin-bottom: 0;
  }
}
.el-col {
  border-radius: 4px;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
.row-bg {
  padding: 10px 0;
}
</style>
