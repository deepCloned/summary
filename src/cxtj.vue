<template>
  <div class="cxtj-center">
  <!-- <div class="cxtj">
    <ul>
      <li @click="showTime=true">
        <img src="../../assets/img/icon-data.png" alt="">
        <span>选择日期</span>
      </li>
      <li @click="showSearc=true">
        <img src="../../assets/img/icon-jtcx.png" alt="">
        <span>条件查询</span>
      </li>
    </ul>
  </div> -->
    <confirm v-model="showTime"
             theme="ios"
             @on-confirm="propValue"
             @on-cancel="cancelValue"
             mask-transition="vux-mask">
      <div class="timebtn">
        <div class="start">
          <span>开始时间</span><input type="text" v-model="startDate" @click="selDate(0)"/>
        </div>
        <div>
          <span>结束时间</span><input type="text" v-model="endDate"  @click="selDate(1)"/>
        </div>
      </div>
    </confirm>
    <!-- 修改弹窗出现位置 -->
    <van-popup
      round
      v-model="showSearc"
      position="right"
      :style="{ width: '6rem', height: '100%' }"
    >
      <div class="search-wrapper">
        <!-- 时间选择 -->
        <div class="date-choose">
          <span class="date-title">时间区间</span>
          <div class="date-gap">
            <span @click="setStartDate" class="date">{{ startDate }}</span>
            <span class="gap">-</span>
            <span @click="setEndDate" class="date">{{ endDate }}</span>
          </div>
          <ul class="date-list">
            <li
              v-for="(item, index) of dateList"
              :key="index"
              :class="['date-item', { active: item.index === activeIndex }]"
              @click="changeActiveIndex(item)"
            >
              {{ item.content }}
            </li>
          </ul>
        </div>

        <!-- 品牌选择 -->
        <div class="common-choose">
          <div class="common-title">品牌选择</div>
          <ul class="common-list">
            <li
              v-for="(item, index) of brandList"
              :key="item.id"
              :class="['common-item', { active: index === activeBrandIndex }]"
              @click="changeActiveBrandIndex(index)"
            >
            <!-- { active: item.index === activeIndex } -->
              {{ item.name }}
            </li>
          </ul>
        </div>
        <div v-if="path !== 'ppcy'" class="common-choose">
          <div class="common-title">平台选择</div>
          <ul class="common-list">
            <li
              v-for="(item, index) of platformList"
              :key="item.id"
              :class="['common-item', { active: item.active === true }]"
              @click="handleSelectPlatform(index)"
            >
              {{ item.name }}
            </li>
          </ul>
        </div>
        <div
          v-if="path === 'fzzl' || path === 'cpph'"
          class="common-choose"
        >
          <div class="common-title">评分类型</div>
          <ul class="common-list">
            <li
              v-for="(item, index) of commentType"
              :key="item.id"
              :class="['common-item', { active: item.active === true }]"
              @click="handleSelectCommentType(index)"
            >
              {{ item.name }}
            </li>
          </ul>
        </div>
        
        <!-- 问题查询和多位剖析查询条件 -->
        <div
          v-if="path === 'dwpx' || path === 'ztfx'"
          class="common-choose"
        >
          <div class="common-title">好差评</div>
          <ul class="common-list">
            <li
              v-for="(item, index) of scoreType"
              :key="item.id"
              :class="['common-item', { active: item.active === true }]"
              @click="handleSelectScoreType(index)"
            >
              {{ item.name }}
            </li>
          </ul>
        </div>
        <div class="confirm-wrap">
          <span @click="handleSearch" class="confirm">确认</span>
        </div>
      </div>
    </van-popup>

    <popup
      
      style="background: #fff">
      <ul class="searcstyle">
        <li class="searchli" v-for="(item,index) in searchList" :key="index">
          <div class="searctile">{{item.title}}</div>
          <div class="searcchek">
            <checker  :type="item.type" v-model="item.type=='checkbox' ? A:B"  default-item-class="item-class" selected-item-class="item-active-class">
              <checker-item :value="el.id" v-for="(el,index) in item.vdata" :key="index"  @on-item-click="checkChage(el)">{{el.name}}</checker-item>
            </checker>
          </div>
        </li>
        <div class="seachbutton">
          <x-button type="primary" @click.native="goSearch" :gradients="['#5185ea', '#5185ea']">确定</x-button>
        </div>
      </ul>
    </popup>

    <!-- 开始时间 -->
    <van-popup
        v-model="startDatePickerShow"
        position="bottom"
        :style="{ width: '100%', height: '40%' }"
    >
      <van-datetime-picker
        type="date"
        v-model="currentDate"
        :min-date="minDate"
        :max-date="maxDate"
        @confirm="confirmStartDateChoose"
        @cancel="cancelStartDateChoose"
      />
    </van-popup>

    <!-- 结束时间 -->
    <van-popup
        v-model="endDatePickerShow"
        position="bottom"
        :style="{ width: '100%', height: '40%' }"
    >
      <van-datetime-picker
        type="date"
        v-model="currentDate"
        :min-date="minDate"
        :max-date="maxDate"
        @confirm="confirmEndDateChoose"
        @cancel="cancelEndDateChoose"
      />
    </van-popup>
  </div>
</template>

<script>
import {
  DatetimePlugin,
  Confirm,
  Popup,
  Checker,
  CheckerItem,
  XButton
} from 'vux';
import Vue from 'vue';
import mixin from '../../assets/mixin.js';
import Tool from '../../assets/Tool.js';
Vue.use(DatetimePlugin)

import DateModel from '../../model/dateModel.js'
import dateUtil from '../../model/CurWeek.js'
import LastMonth from '../../model/lastMonth.js'

const dateModel = new DateModel()
const lastDay = dateModel.getYesterday()

const lastWeekInfo = dateUtil.getLastWeek(dateModel.now)

const lastMonth = new LastMonth()
const lastMonthInfo = lastMonth.getLastMonth(dateModel.now)

export default {
  name: 'HelloWorld',
  mixins: [mixin],
  data () {
    return {
      searchData: {},
      value: [],
      showTime: false,
      startDate: '',
      endDate: '',
      checkeDataA: '',
      checkeDataB: '',
      timeobj: {},
      searchobj: {},
      A: [],
      B: '',
      btnA: false,
      btnB: false,
      // 品牌列表，从缓存中取出来
      brandList: [],
      activeBrandIndex: 0,
      // 当前路由 path
      path: '',
      // 日期选择
      dateList: [
        {
          content: '昨天',
          index: 0,
          startDate: lastDay,
          endDate: lastDay,
        },
        {
          content: '上星期',
          index: 1,
          startDate: lastWeekInfo.start,
          endDate: lastWeekInfo.end,
        },
        {
          content: '上月',
          index: 2,
          startDate: lastMonthInfo.start,
          endDate: lastMonthInfo.end,
        },
        // {
        //   content: '本月',
        //   index: 3
        // }
      ],
      activeIndex: 0,
      // 日期相关
      currentDate: new Date(),
      minDate: new Date('2016-01-01'),
      maxDate: new Date(),
      startDate: lastDay,
      endDate: lastDay,
      startDatePickerShow: false,
      endDatePickerShow: false,
    }
  },
  props: {
    searchList: Array,
    type: String,
    // 平台列表
    platformList: {
      type: Array,
      default() {
        return []
      }
    },
    // 评分类型
    commentType: {
      type: Array,
      default() {
        return []
      }
    },
    // 好差评
    scoreType: {
      type: Array,
      default() {
        return []
      }
    },
    // 是否展示搜索弹窗
    showSearchBox: {
      type: Boolean
    }
  },
  computed: {
    showSearc: {
      get() {
        return this.showSearchBox
      },
      set(val) {
        this.$emit('closeSearchBox', val)
      }
    }
  },
  components: {
    DatetimePlugin,
    Confirm,
    Popup,
    Checker,
    CheckerItem,
    XButton
  },
  methods: {
    init () {
      if (
        this.type === 'qykb' ||
        this.type === 'ztfx' ||
        this.type === 'cpph' ||
        this.type === 'fzzl' ||
        this.type === 'xjzl'
      ) {
        this.startDate = Tool.monthStartDate()
        this.endDate = Tool.format(new Date(), '20yy-MM-dd')
        this.timeobj.startDate = this.startDate
        this.timeobj.endDate = this.endDate
        this.$emit('timeFn', this.timeobj)
        if (this.startDate && this.endDate) {
          this.showSearc = true
        }
      }
      if (this.type === 'cpph') {
        this.B = '00';
        this.searchobj.leixing = {
          id: '00',
          prop: 'leixing',
          name: '综合'
        }
        this.$emit('searchFn', this.searchobj)
        // console.log(this.searchobj.leixing.id)
      }
      if (this.type === 'ztfx') {
        this.B = '01';
        this.searchobj.pinglun = {
          id: '01',
          prop: 'pinglun',
          name: '差评'
        }
        this.$emit('searchFn', this.searchobj)
        // console.log(this.searchobj.pinglun.id)
      }
    },
    selDate (index) {
      let vm = this
      this.$vux.datetime.show({
        cancelText: '取消',
        confirmText: '确定',
        format: 'YYYY-MM-DD',
        value: Tool.format(new Date(), '20yy-MM-dd'),
        endDate: Tool.format(new Date(), '20yy-MM-dd'),
        onConfirm (val) {
          if (index === 0) {
            vm.startDate = val
          } else {
            vm.endDate = val
          }
        },
        onShow () {},
        onHide () {}
      })
    },
    cancelValue () {
      this.startDate = '';
      this.endDate = '';
    },
    /* 点击确定----查询时间 */
    propValue () {
      this.timeobj.startDate = this.startDate
      this.timeobj.endDate = this.endDate
      this.$emit('timeFn', this.timeobj)
      this.btnA = !this.btnA
      this.$emit('btnA', this.btnA)
    },
    /* 点击确定----查询条件 */
    goSearch () {
      this.showSearc = false
      console.log('searchObj is', this.searchobj)
      // console.log(this.searchobj)
      this.$emit('searchFn', this.searchobj)
      this.btnB = !this.btnB
      this.$emit('btnB', this.btnB)
    },
    checkChage (el) {
      let type = this.type
      if (type === 'qykb') {
        this.qykb(el)
      } else if (type === 'ztfx') {
        this.ztfx(el)
      } else if (type === 'cpph') {
        this.cpph(el)
      } else if (type === 'fzzl') {
        this.fzzl(el)
      } else if (type === 'xjzl') {
        this.xjzl(el)
      }
    },
    // 点击事件更改平台选择
    changeActiveBrandIndex(index) {
      this.activeBrandIndex = index
    },
    // 点击选择多个平台
    handleSelectPlatform(index) {
      // 选择平台，发布事件告知父组件，告知点击的索引
      this.$emit('selectPlatform', index)
    },
    // 选择评分类型
    handleSelectCommentType(index) {
      this.$emit('selectCommentType', index)
    },
    // 选择好差评
    handleSelectScoreType(index) {
      this.$emit('selectScoreType', index)
    },
    // 点击确认，搜索
    handleSearch() {
      this.$emit('closeSearchBox', false)
      // 获取选中品牌
      let searchList = []
      let brand = this.brandList[this.activeBrandIndex]
      let date = {
        startDate: this.startDate,
        endData: this.endDate,
      }
      // 传入时间
      searchList.push({
        title: 'date',
        date,
      })
      searchList.push({
        title: 'brand',
        brand,
      })
      this.$emit('searchFn', searchList)
      this.btnB = !this.btnB
      this.$emit('btnB', this.btnB)
    },
    /**
     * 更改日期选中项
     * 昨天
     * 上星期
     * 上月
     */
    changeActiveIndex(item) {
      this.activeIndex = item.index
      this.startDate = item.startDate
      this.endDate = item.endDate
    },
    setStartDate() {
      this.startDatePickerShow = true
    },
    setEndDate() {
      this.endDatePickerShow = true
    },
    cancelStartDateChoose() {
      this.startDatePickerShow = false
    },
    cancelEndDateChoose() {
      this.endDatePickerShow = false
    },
    confirmStartDateChoose(value) {
      // 随便设的，目的是取消下面的选中效果
      this.activeIndex = 999
      this.startDatePickerShow = false
      const startDate = dateModel.formateDate(value)
      this.startDate = startDate
    },
    confirmEndDateChoose(value) {
      this.activeIndex = 999
      this.endDatePickerShow = false
      const endDate = dateModel.formateDate(value)
      this.endDate = endDate
    },
  },
  created() {
    /**
     * 品牌在首页中请求过
     * 直接从缓存中取
     */
    let brandList = JSON.parse(sessionStorage.getItem('brandList'))
    this.brandList = brandList
    
    // 赋值当前路径的 path 控制搜索条件的显示和隐藏
    this.path = this.$route.path.slice(1)
  },
  mounted () {
    this.init()
    if ((this.type !== 'cpph') & (this.type !== 'ztfx')) {
      this.searchobj = {}
    }
    // console.log('searchList is', this.searchList)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less"  type="text/less">
.cxtj-center {
  width: 100%;
  text-align: center;
  .cxtj {
    position: absolute;
    z-index: 1;
    left: 0;
    right: 0;
    margin: auto;
    display: inline-block;
    width: 7rem;
    height: 0.64rem;
    background: #fff;
    border-radius: 0.1rem;
    box-shadow: 1px 2px 10px rgba(0, 0, 0, 0.1);
    ul {
      width: 100%;
      display: flex;
      li {
        font-size: 0.3rem;
        width: 50%;
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: row;
        line-height: 0.64rem;
        border-right: 1px #eeeeee dashed;
        img {
          width: 0.4rem;
          margin-right: 0.18rem;
        }
      }
      li:last-child {
        border-right: none;
      }
    }
  }
  .timebtn {
    .start {
      margin-bottom: 0.5rem;
    }
    span {
      display: inline-block;
      margin-right: 0.2rem;
    }
    input {
      display: inline-block;
      width: 3rem;
      margin-top: -3px;
      border: none;
      font-size: inherit;
      background: transparent;
      vertical-align: middle;
      &::-webkit-search-cancel-button {
        -webkit-appearance: none;
      }
      border-bottom: 0.01rem solid #dcdcdc;
    }
  }
  .searcstyle {
    box-sizing: border-box;
    .searchli {
      padding-top: 0.39rem;
      padding-left: 0.42rem;
      overflow: hidden;
      padding-bottom: 0.34rem;
      border-bottom: 1px solid #dcdcdc;
      .searctile {
        float: left;
        font-size: 0.3rem;
        color: #888787;
        margin-bottom: 0.18rem;
      }
      .searcchek {
        clear: both;
        .item-class {
          // width: 2.15rem;
          // min-height: 0.87rem;
          width: 1.5rem;
          min-height: 0.65rem;
          background: #f4f4f4;
          text-align: center;
          // line-height: 0.87rem;
          line-height: 0.65rem;
          float: left;
          border-radius: 0.08rem;
          // font-size: 0.3rem;
          font-size: 0.28rem;
          color: #000;
          // margin-right: 0.14rem;
          margin-right: 0.25rem;
          margin-bottom: 0.2rem;
        }
        .item-active-class {
          // width: 2.15rem;
          // height: 0.87rem;
          width: 1.5rem;
          min-height: 0.65rem;
          background: #5185ea;
          text-align: center;
          // line-height: 0.87rem;
          line-height: 0.65rem;
          float: left;
          border-radius: 0.08rem;
          // font-size: 0.3rem;
          font-size: 0.28rem;
          color: #fff;
          // margin-right: 0.14rem;
          margin-right: 0.25rem;
          margin-bottom: 0.2rem;
        }
      }
    }
  }
  .seachbutton {
    padding: 0.2rem 0.2rem;
  }
}
// 品牌选择样式
.search-wrapper{
    box-sizing: border-box;
    width: 6rem;
    height: 100%;
    padding: .2rem .4rem .5rem;
    background: #fff;
    .date-choose{
      text-align: left;
      .date-title{
        height: .5rem;
        font-size: .28rem;
        text-align: left;
        color: #666;
        padding-left: .2rem;
      }
      .date-gap{
        display: flex;
        align-items: center;
        margin-top: .2rem;
        height: .5rem;
        .date{
          width: 2.25rem;
          height: .48rem;
          font-size: .23rem;
          background: #f8f8f8;
          border-radius: .48rem;
          text-align: center;
          line-height: .48rem;
        }
        .gap{
          width: .5rem;
          height: .5rem;
          text-align: center;
          line-height: .5rem;
        }
      }
      .date-list{
        display: flex;
        flex-wrap: wrap;
        .date-item{
          box-sizing: border-box;
          display: flex;
          align-items: center;
          justify-content: center;
          font-size: .24rem;
          width: 1.4rem;
          height: .5rem;
          background: #f8f8f8;
          border-radius: .5rem;
          margin-top: .2rem;
          transition: all .2s ease-in-out;
          &:nth-of-type(2){
            margin-left: .4rem;
            margin-right: .4rem;
          }
          &.active{
            border: 1px solid #5184e9;
            color: #5184e9;
          }
        }
      }
      .confirm-wrap{
        display: flex;
        justify-content: flex-end;
        height: .6rem;
        margin-top: .3rem;
        .confirm{
          display: inline-block;
          background: #5184e9;
          border-radius: .5rem;
          color: #fff;
          width: 1.4rem;
          height: .5rem;
          text-align: center;
          line-height: .5rem;
          font-size: .24rem;
        }
      }
    }
    .common-choose{
      position: relative;
      padding-bottom: .3rem;
      &:nth-of-type(1){
        &:after{
          position: absolute;
          left: -.5rem;
          bottom: 0;
          display: block;
          content: "";
          width: 6rem;
          height: 0;
          border-top: 1px solid #ededed;
        }
      }
      &:nth-of-type(2){
        margin-top: .2rem;
      }
      .common-title{
        margin-top: .1rem;
        height: .5rem;
        font-size: .28rem;
        color: #666;
        padding-left: .2rem;
        text-align: left;
      }
      .common-gap{
        display: flex;
        align-items: center;
        margin-top: .2rem;
        height: .5rem;
        .common{
          width: 2.25rem;
          height: .48rem;
          font-size: .23rem;
          background: #f8f8f8;
          border-radius: .48rem;
          text-align: center;
          line-height: .48rem;
        }
        .gap{
          width: .5rem;
          height: .5rem;
          text-align: center;
          line-height: .5rem;
        }
      }
      .common-list{
        display: flex;
        flex-wrap: wrap;
        .common-item{
          box-sizing: border-box;
          display: flex;
          align-items: center;
          justify-content: center;
          font-size: .24rem;
          height: .5rem;
          background: #f8f8f8;
          border-radius: .5rem;
          margin-top: .2rem;
          transition: all .2s ease-in-out;
          padding-right: .2rem;
          padding-left: .2rem;
          margin-left: .2rem;
          &.active{
            box-sizing: border-box;
            border: 1px solid #5184e9;
            color: #5184e9;
          }
        }
      }
      
    }
    .confirm-wrap{
        display: flex;
        justify-content: flex-end;
        height: .6rem;
        margin-top: .3rem;
        padding-right: .2rem;
        .confirm{
          display: inline-block;
          background: #5184e9;
          border-radius: .5rem;
          color: #fff;
          width: 1.4rem;
          height: .5rem;
          text-align: center;
          line-height: .5rem;
          font-size: .24rem;
        }
      }
  }
</style>
