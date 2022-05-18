<template>
  <div class="container" style="overflow: hidden; padding-left: 20px" type="flex">
    <div type="flex">
      <el-row :gutter="20" style="margin-bottom: 10px">
        <el-col :span="12">
          <div
            class="grid-content bg-purple"
            style="
              color: pink;
              line-height: 30px;
              font-size: 12px;
              padding-left: 10px;
            "
          >
            说明：目前支持学科和关键字条件筛选
          </div>
        </el-col>
        <el-col :span="12">
          <div class="grid-content bg-purple" style="padding-left: 540px">
            <el-button type="success" icon="el-icon-edit">新增试题</el-button>
          </div>
        </el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form>
              <el-form-item label="学科" style="padding-left: 28px" >
                <el-select
                  v-model="form.region"
                  placeholder="请选择"
                  style="width: 86%"
                  @change="fnChange(form.region)"
                >
                  <el-option  v-for="item in SubjectList" :key="item.valve" :label="item.label" :value="item.value"></el-option>
                </el-select>
              </el-form-item>
            </el-form>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form>
              <el-form-item label="二级目录">
                <el-select
                  v-model="form.Secondary"
                  placeholder="请选择"
                  style="width: 75%"
                  @change="fnChange1(form.Secondary)"
                >
                  <el-option v-for="item in SubjectList2" :key="item.valve" :label="item.label" :value="item.value"></el-option>
                </el-select>
              </el-form-item>
            </el-form>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form>
              <el-form-item label="标签">
                <el-select
                  v-model="form.Label"
                  placeholder="请选择"
                  style="width: 87%"
                >
                  <el-option v-for="item in SubjectList3" :key="item.valve" :label="item.label" :value="item.value"></el-option>
                </el-select>
              </el-form-item>
            </el-form>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form>
              <el-form-item label="关键字">
                <el-input  placeholder="根据题干搜索" v-model="form.questions" style="width: 80%"></el-input>
              </el-form-item>
            </el-form>
          </div>
        </el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form>
              <el-form-item label="试题类型">
                <el-select
                  v-model="form.Keywords"
                  placeholder="请选则"
                  style="width: 69%"
                >
                  <el-option label="单选" value="单选"></el-option>
                  <el-option label="多选" value="多选"></el-option>
                  <el-option label="简答" value="简答"></el-option>
                </el-select>
              </el-form-item>
            </el-form>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form>
              <el-form-item label="难度" style="padding-left: 28px">
                <el-select
                  v-model="form.Difficulty"
                  placeholder="请选择"
                  style="width: 86%"
                >
                  <el-option label="简单" value="简单"></el-option>
                  <el-option label="一般" value="一般"></el-option>
                  <el-option label="困难" value="困难"></el-option>
                </el-select>
              </el-form-item>
            </el-form>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form>
              <el-form-item label="方向">
                <el-select
                  v-model="form.Direction"
                  placeholder="请选择"
                  style="width: 86%"
                >
                  <el-option label="o2o" value="o2o"></el-option>
                  <el-option label="外包服务" value="外包服务"></el-option>
                  <el-option label="企业服务" value="企业服务"></el-option>
                  <el-option label="互联网金融" value="互联网金融"></el-option>
                  <el-option label="企业咨询" value="企业咨询"></el-option>
                  <el-option label="互联网" value="互联网"></el-option>
                  <el-option label="电子商务" value="电子商务"></el-option>
                  <el-option label="其他" value="其他"></el-option>
                </el-select>
              </el-form-item>
            </el-form>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form>
              <el-form-item label="录入人">
                <el-select
                  v-model="form.Entered"
                  placeholder="请选择"
                  style="width: 80%"
                >
                  <el-option v-for="item in enteredList" :key="item.valve" :label="item.username" :value="item.id"></el-option>
                </el-select>
              </el-form-item>
            </el-form>
          </div>
        </el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form>
              <el-form-item label="题目备注">
                <el-input v-model="form.remarks" style="width: 75%"></el-input>
              </el-form-item>
            </el-form>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form>
              <el-form-item label="企业简称">
                <el-input v-model="form.Enterprise" style="width: 75%"></el-input>
              </el-form-item>
            </el-form>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple">
            <el-form>
              <el-form-item label="城市" >
                <el-select
                  class="filter-item"
                  style="width: 130px"
                  v-model="formBase.province"
                  @keyup.enter="handleFilter"
                  @change="handleProvince"
                  filterable
                >
                  <el-option
                    v-for="item in citySelect.province"
                    :key="item"
                    :label="item"
                    :value="item"
                  ></el-option>
                </el-select>
                <el-select
                  class="filter-item"
                  style="width: 130px"
                  v-model="formBase.city"
                  @keyup.enter="handleFilter"
                  filterable
                >
                  <el-option
                    v-for="item in citySelect.cityDate"
                    :key="item"
                    :label="item"
                    :value="item"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-form>
          </div>
        </el-col>
        <el-col :span="6">
          <div class="grid-content bg-purple" style="padding-left: 160px">
            <template>
              <el-button>清除</el-button>
              <el-button type="primary" @click="searchInput()">搜索</el-button>
            </template>
          </div>
        </el-col>
      </el-row>
    </div>
    <el-row>
      <el-tabs v-model="tabPane" type="border-card" @tab-click="tabClick(tabPane)">
        <el-tab-pane label="全部" name="全部"/>
        <el-tab-pane label="待审核" name="待审核"/>
        <el-tab-pane label="已审核" name="已审核"/>
        <el-tab-pane label="已拒绝" name="已拒绝"/>
      </el-tabs>
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
        <el-table-column fixed prop="number" label="试题编号" width="120">
        </el-table-column>
        <el-table-column prop="subject" label="学科" width="100">
        </el-table-column>
        <el-table-column prop="catalog" label="目录" width="120">
        </el-table-column>
        <el-table-column prop="questionType" label="题型" width="120">
          <template v-slot="{row}">
            <div v-html="row.questionType ==='1'?'单选':(row.questionType==='2'?'多选':'简答')"></div>
          </template>
        </el-table-column>
        <el-table-column prop="question" label="题干" width="300">
          <template v-slot="{row}">
            <div  v-html="row.question "></div>
          </template>
        </el-table-column>
        <el-table-column prop="addDate" label="录入时间" width="160">
          <template v-slot="{row}">
            <span> {{row.addDate | parseTimeByString }}</span>
          </template>
        </el-table-column>

        <el-table-column prop="difficulty" label="难度" width="120">
          <template v-slot="{row}">
            <div  v-html="row.difficulty ==='1'?'简单':(row.difficulty==='2'?'一般':'困难')"></div>
          </template>
        </el-table-column>
        <el-table-column prop="creator" label="录入人" width="100"> </el-table-column>
        <el-table-column prop="chkState" label="审核状态" width="100">
          <template v-slot="{row}">
            <div  v-html="row.chkState =='0'?'待审核':(row.chkState=='1'?'已审核':'已拒绝')"></div>
          </template>
        </el-table-column>
        <el-table-column prop="chkRemarks" label="审核意见" width="100"> </el-table-column>
        <el-table-column prop="chkUser" label="审核人" width="100"> </el-table-column>
        <el-table-column prop="publishState" label="发布状态" width="100">
          <template v-slot="{row}">
            <div  v-html="row.publishState ===0?'待发布':'已发布'"></div>
          </template></el-table-column>
        <el-table-column
          fixed="right"
          label="操作"
          width="200">
          <template v-slot="{ row }">
            <el-button type="text" size="mini"  >预览</el-button>
            <el-button type="text" size="mini" :disabled="row.chkState == 1" @click="auditBut(row)">审核</el-button>
            <el-button type="text" size="mini"   :disabled="row.publishState == 1" >修改</el-button>
            <el-button type="text" size="mini"   @click="clickCheck(row)">{{row.publishState == 1 ? '下架' : '上架' }}</el-button>
            <el-button type="text" size="mini" :disabled="row.publishState ==1" @click="clickDelete(row)" >删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-row>
    <el-row class="block" style="padding-left:950px">
      <el-pagination
        layout="prev,pager,next,sizes,jumper"
        :page-sizes="[3, 5, 7, 10]"
        :page-size="pageParams.pagesize"
        :total="total"
        :current-page.sync="curPage"
        @current-change="hCurrentChange"
        @size-change="hSizeChange"
      />
    </el-row>
    <el-dialog type="flex" :visible="dialogShow" @close="dialogShow=false" title="题目审核" width="400px"  >
      <el-row>
        <template>
          <el-radio v-model="radio" label="1">通过</el-radio>
          <el-radio v-model="radio" label="2">拒绝</el-radio>
        </template>
      </el-row>
     <el-row style="padding-top: 40px;padding-bottom: 40px">
       <el-input
         type="textarea"
         :rows="2"
         placeholder="请输入内容"
         v-model="textarea">
       </el-input>
     </el-row>
      <el-row style="padding-left: 200px">
        <el-button @click="dialogShow=false">取消</el-button>
        <el-button type="primary" @click="question()">确认</el-button>
      </el-row>
    </el-dialog>
  </div>
</template>

<script>
import { detail, simple } from '@/api/hmmm/subjects'
import { simple1 } from '@/api/hmmm/directorys'
import { citys, provinces } from '@/api/hmmm/citys'
import { simple3 } from '@/api/hmmm/tags'
import { simple4 } from '@/api/base/users'
import { choice, choiceAdd, choiceCheck, choicePublish, list, remove } from '@/api/hmmm/questions'
export default {
  data () {
    return {
      // 删除 和 修改
      disabledShow1: false,
      // 判断是否可以审核
      disabledShow: true,
      // 试题审核id
      questionID: '',
      // 原因
      textarea: '',
      // 通过 拒绝
      radio: '',
      // 显示审核
      dialogShow: false,
      // 切换值
      tabPane: '全部',
      pageParams: {
        // 页码  每页几条
        page: 1,
        pagesize: 5
      },
      curPage: 0, // 当前页数
      list1: [], // 角色列表
      total: 0, // 一共几条数据
      formBase: {
        province: '',
        city: ''
      },
      citySelect: {
        province: [],
        cityDate: []
      },
      // 录入人list
      enteredList: [],
      // 获取学科列表
      SubjectList: [],
      // 二级学科列表
      SubjectList2: [],
      // 三级标签
      SubjectList3: [],
      form: {
        // 学科
        region: '',
        // 二级目录
        Secondary: '',
        // 标签
        Label: '',
        // 关键字
        questions: '',
        // 试题类型
        questionType: '',
        // 难度
        difficulty: '',
        // 方向
        Direction: '',
        // 录入人
        Entered: '',
        // 题目备注
        remarks: '',
        // 企业简称
        Enterprise: ''

      },
      tableData: [
        {
          chkRemarks: '',
          publishState: '',
          number: '',
          subject: '',
          catalog: '',
          catalogID: '',
          question: '',
          addDate: '',
          creatorID: '',
          creator: ''
        }]
    }
  },
  created () {
    // 获取学科列表
    this.simpleList()
    // 获取省
    this.getCityData()
    // 获取录入人
    this.EnteredList()
    // 获取表格
    this.tableDataList()
  },
  methods: {
    // 点击确认
    async question () {
      const res = await choiceCheck({
        id: this.questionID,
        chkState: Number(this.radio),
        chkRemarks: this.textarea
      })
      this.$message.success('题目审核成功!')
      await this.tableDataList()
      this.dialogShow = false
    },
    // 题目审核
    auditBut (row) {
      // 显示弹窗
      this.dialogShow = true
      console.log(row.id)
      this.questionID = row.id
    },
    // 点击切换
    async tabClick (id) {
      if (id === '全部') {
        await this.tableDataList()
      } else if (id === '待审核') {
        const res = await choice({
          page: this.pageParams.page,
          pagesize: this.pageParams.pagesize,
          chkState: 0
        })
        this.total = res.data.counts
        this.tableData = res.data.items
        console.log('待审核')
      } else if (id === '已审核') {
        const res = await choice({
          page: this.pageParams.page,
          pagesize: this.pageParams.pagesize,
          chkState: 1
        })
        this.total = res.data.counts
        this.tableData = res.data.items
        console.log('已审核')
      } else {
        console.log('已拒绝')
        const res = await choice({
          page: this.pageParams.page,
          pagesize: this.pageParams.pagesize,
          chkState: 2
        })
        this.total = res.data.counts
        this.tableData = res.data.items
      }
    },
    // 搜索        form.region
    async searchInput () {
      const res = await choice({
        page: this.pageParams.page,
        pagesize: this.pageParams.pagesize,
        // 学科
        subjectID: this.form.region,
        // 二级目录
        catalogID: this.form.Secondary,
        // 三级标签
        tags: this.form.Label,
        //  关键字
        keyword: this.form.questions
      })
      this.total = res.data.counts
      this.tableData = res.data.items
      console.log(res)
    },
    // 点击对号上架
    // 1上架 0下架
    async clickCheck (row) {
      if (row.publishState == 0) {
        await choicePublish({
          id: row.id,
          publishState: 1
        })
      } else {
        await choicePublish({
          id: row.id,
          publishState: 0
        })
      }
      console.log(row)
      // 从新获取数据
      await this.tableDataList()
      await this.$confirm(row.publishState === 0 ? '您确认上架这道题目!' : '您确认下架这道题目!', '提示', { type: 'warning' })
      this.$message.success(row.publishState === 0 ? '上架成功!' : '下架成功!')
      this.disabledShow1 = true
    },
    // pageSize 改变时会触发
    // 每页几条
    // 用户调整了每页显示的条数
    hSizeChange (pagesize) {
      // 1. 更新每页条数
      this.pageParams.pagesize = pagesize
      // 2. 重发请求
      this.tableDataList()
    },
    hCurrentChange (curPage) {
      // alert(curPage)
      // 1. 更新页码
      this.pageParams.page = curPage
      // 2. 重发请求
      this.tableDataList()
    },
    // 表格删除
    async  clickDelete (id) {
      await remove({
        id: id.id
      })
      await this.$confirm('确认删除!')
      this.$message.success('删除成功!')
      await this.tableDataList()
    },
    // 获取表格
    async tableDataList () {
      const res = await choice({
        page: this.pageParams.page,
        pagesize: this.pageParams.pagesize
      })
      console.log(res)
      this.total = res.data.counts
      this.tableData = res.data.items
      // console.log(res.data.items[1].chkState)
    },
    // 获取录入人
    async EnteredList () {
      const res4 = await simple4()
      this.enteredList = res4.data
    },
    // 获取学科列表
    async simpleList () {
      const res = await simple()
      // console.log(res)
      this.SubjectList = res.data
    },
    // 二级目录
    async fnChange (id) {
      const res1 = await simple1({
        subjectID: id
      })
      this.SubjectList2 = res1.data
      this.form.Secondary = ''
      this.form.Label = ''
    },
    // 三级标签
    async  fnChange1 (id) {
      const res3 = await simple3({
        subjectID: id
      })
      this.SubjectList3 = res3.data
    },
    // 获取省
    getCityData: function () {
      this.citySelect.province = provinces()
    },
    // 选省获取到市
    handleProvince: function (e) {
      this.citySelect.cityDate = citys(e)
      this.formBase.city = this.citySelect.cityDate[0]
    }
  }
}
</script>

<style scoped lang="less">
.el-row {
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
  background-color: #f9fafc;
}
.el-pagination__jump{
  margin-left: 0 !important;
}

 .el-tabs {
   height: 41px!important;
   overflow: hidden!important;
 }

.el-tabs--border-card{
  border: 0;
}

</style>
