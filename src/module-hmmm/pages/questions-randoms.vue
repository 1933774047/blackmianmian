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
        <template v-slot="{row}">
          <div v-html="row.questionType ==='1'?'单选':(row.questionType==='2'?'多选':'简答')"></div>
        </template>
      </el-table-column>
      <el-table-column prop="questionIDs" label="题目编号" width="200" >
        <template v-slot="{row}">
          <div v-html="item.number" v-for="item in row.questionIDs" style="color: rgb(0, 153, 255)" @click="clickView(item)"></div>
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

    <el-dialog title="题目预览" :visible="PopUps" @close="PopUps=false ,showHide=false ">
      <el-row :gutter="50" style="line-height:36px">
        <el-col :span="6"><div class="grid-content bg-purple">【题型】 :{{form.questionType==='1'?'单选':(form.questionType==='2'?'多选':'简答')}}</div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple">【编号】 :{{form.id}}</div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple">【难度】 :{{form.difficulty ==='1'?'简单':(form.difficulty==='2'?'一般':'困难')}}</div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple">【标签】 :{{form.tags}}</div></el-col>
      </el-row>
      <el-row :gutter="50" style="line-height:36px">
        <el-col :span="6"><div class="grid-content bg-purple">【学科】 :{{form.subjectName}}</div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple">【目录】 :{{form.directoryName}}</div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple">【方向】 :{{form.direction}}</div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple"></div></el-col>
      </el-row>
      <el-row style="border-bottom: 1px solid #333 ;border-top: 1px solid #333">
        <div style="padding-top: 10px; padding-bottom: 10px" >【题干】: <span style="color: #409EFF" v-html="form.question"></span> </div>
        <div  style="padding-top: 10px;">{{form.questionType==='1'?'单选选项：（以下选中的选项为正确答案）':(form.questionType==='2'?'多选选项：（以下选中的选项为正确答案）':'简答选项：（以下选中的选项为正确答案）')}}  </div>
        <div v-if="true" >
          <el-checkbox-group v-model="options">
            <el-checkbox   v-for="item in form.options" :label="item.title" true-label="item." :key="item.id" style="display: block; padding-top: 10px" >{{item.title}}</el-checkbox >
          </el-checkbox-group>
        </div>
      </el-row>
      <el-row style="padding-top: 10px; padding-bottom: 10px">
        <div >【参考答案】:   <el-button @click="clickChowHide()" type="danger" size="medium">视频答案预览</el-button>
        </div>
        <div class='demo' v-show="showHide">
          <div class='demo' >
            <video-player class="video-player vjs-custom-skin"
                          ref="videoPlayer"
                          :playsinline="true"
                          :options="playerOptions">
            </video-player>
          </div>
        </div>
      </el-row>
      <el-row style="border-bottom: 1px solid #333 ;border-top: 1px solid #333">
        <div style="padding-top: 20px; padding-bottom: 20px">【答案解析】: <span style="display: inline-block" v-html="form.answer"></span>  </div>
      </el-row>
      <el-row style="border-bottom: 1px solid #333 ">
        <div style="padding-top: 10px; padding-bottom: 20px">【题目备注】:{{form.remarks}}   </div>
      </el-row>
      <el-row style="padding-left:90%;padding-top: 10px;">
        <el-button  type="primary" @click="PopUps=false">关闭</el-button>
      </el-row>
    </el-dialog>

    </div>
</template>

<script>
import { detail, randoms, removeRandoms } from '@/api/hmmm/questions'

export default {
  data () {
    return {
      //
      options: [],
      // 视频
      showHide: false,
      // 弹窗
      PopUps: false,
      playerOptions: {
        playbackRates: [0.5, 1.0, 1.5, 2.0], // 可选的播放速度
        autoplay: false, // 如果为true,浏览器准备好时开始回放。
        muted: false, // 默认情况下将会消除任何音频。
        loop: false, // 是否视频一结束就重新开始。
        preload: 'auto', // 建议浏览器在<video>加载元素后是否应该开始下载视频数据。auto浏览器选择最佳行为,立即开始加载视频（如果浏览器支持）
        language: 'zh-CN',
        aspectRatio: '16:9', // 将播放器置于流畅模式，并在计算播放器的动态大小时使用该值。值应该代表一个比例 - 用冒号分隔的两个数字（例如"16:9"或"4:3"）
        fluid: true, // 当true时，Video.js player将拥有流体大小。换句话说，它将按比例缩放以适应其容器。
        sources: [{
          type: 'video/mp4', // 类型
          src: 'http://vjs.zencdn.net/v/oceans.mp4' // url地址
        }],
        poster: '', // 封面地址
        notSupportedMessage: '此视频暂无法播放，请稍后再试', // 允许覆盖Video.js无法播放媒体源时显示的默认信息。
        controlBar: {
          timeDivider: true, // 当前时间和持续时间的分隔符
          durationDisplay: true, // 显示持续时间
          remainingTimeDisplay: false, // 是否显示剩余时间功能
          fullscreenToggle: true // 是否显示全屏按钮
        }
      },
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
      ],
      form: {
        options: ['JAVA2 EE'],
        directoryName: '',
        answer: '',
        videoURL: '',
        question: '',
        id: '',
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
        Enterprise: '',
        subjectName: ''

      }
    }
  },
  created () {
    this.randomsList()
  },
  methods: {
    // changeFn
    changeFn (value) {
      console.log(value)
    },
    // 视频
    clickChowHide () {
      this.showHide = true
    },
    // 预览
    async clickView (row) {
      console.log(row)
      this.PopUps = true
      try {
        const res = await detail({
          id: row.id
        })
        console.log(res.data.options)
        this.form.questionType = res.data.questionType
        this.form.id = res.data.id
        this.form.difficulty = res.data.difficulty
        this.form.tags = res.data.tags
        this.form.subjectName = res.data.subjectName
        this.form.directoryName = res.data.directoryName
        this.form.direction = res.data.direction
        this.form.question = res.data.question
        this.form.remarks = res.data.remarks
        this.form.videoURL = res.data.videoURL
        this.form.answer = res.data.answer
        this.form.options = res.data.options

        //  弹出层的选中状态
        // this.options = this.form.options.filter(item => item.isRight === 0).forEach(item => this.options.push(item.title))
      } catch (error) {
        console.log(error)
        this.$message.warning('该题目编号没数据！')
      }
    },
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
      console.log(this.tableData)
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
