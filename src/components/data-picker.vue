<!--
 * @Description: In User Settings Edit
 * @Author: your name
 * @Date: 2019-09-14 20:40:19
 * @LastEditTime: 2019-09-15 09:58:36
 * @LastEditors: Please set LastEditors
 -->
<template>
  <div v-click-outside>
    <input type="text" :value="formatDate">
    <div class="pannel" v-if="isVisible">
      <div class="pannel-nav">
        <span>&lt;</span>
        <span @click="prevMonth">&lt;&lt;</span>
        <span>{{time.year}}年</span>
        <span>{{time.month+1}}月</span>
        <span @click="nextMonth">&gt;&gt;</span>
        <span>&gt;</span>
      </div>
      <div class="pannle-content">
        <div class="days">
          <!-- t头部周 -->
          <span v-for="j in 7" :key="`_`+j" class="cell">
            {{weekDays[j-1]}}
          </span>
          <!-- 直接列出一个6*7 一个列表 99乘法表 -->
          <div v-for="i in 6" :key="i">
            <!-- //notCurrentMonth:判断是不是当月，不是当月就变灰 -->
            <!-- chooseDate:选择日期的方法 -->
            <span v-for="j in 7" :key="j" class="cell cell-days" @click="chooseDate(visibleDays[(i-1)*7+(j-1)])" :class="[
            {notCurrentMonth:!isCurrentMonth(visibleDays[(i-1)*7+(j-1)])},
            {today:isToday(visibleDays[(i-1)*7+(j-1)])},
            {select:isSelect(visibleDays[(i-1)*7+(j-1)])}
            ]">

              {{visibleDays[(i-1)*7+(j-1)].getDate()}}
            </span>
          </div>
        </div>
      </div>
      <div class="pannel-footer">
        今天
      </div>
      <!-- 事件委托 -->
    </div>
  </div>
</template>
<script>
import * as  utils from './utils'
export default {
  directives: {
    clickOutside: {
      //指令的生命周期
      bind (el, binding, vnode) { //content
        //把事件绑定给doucument上，看一下点击的是否是当前这个元素
        let handler = (e) => {
          // console.log(e.target)
          //判断当前上下文是否包含该元素
          if (el.contains(e.target)) {
            //判断是否当前面板已经显示出来
            if (!vnode.context.isVisible) {
              vnode.context.focus();
              console.log('focus')
            }
            // vnode.context.focus();
            // console.log('focus')
          } else {
            if (vnode.context.isVisible) {
              vnode.context.blur();
              console.log('blur')
            }
            // vnode.context.blur();
            // console.log('blur')
          }
        }
        el.bandler = handler;
        document.addEventListener('click', handler)
      },
      unbind (el) {
        document.removeEventListener('click', el.handler)
      }
    }
  },
  data () {
    let { year, month } = utils.getYearMonthDay(this.value);
    return {
      weekDays: ['日', '一', '二', '三', '四', '五', '六'],
      time: { year, month },
      isVisible: false //这个变量是用来控制面板是否可见
    }
  },
  props: {
    value: {
      type: Date,
      default: () => new Date  //返回默认值必须是一个函数类型{} []
    }
  },
  methods: {
    focus () {
      this.isVisible = true; //显示面板
    },
    blur () {
      this.isVisible = false; //隐藏面板
    },
    //判断是不是当月this.value
    isCurrentMonth (Date) {
      //我现在知道当前用户传入的值 2018 9 你8 2018 4 28 /2018 6 8
      let { year, month } = utils.getYearMonthDay(utils.getDate(this.time.year, this.time.month, 1));
      let { year: y, month: m } = utils.getYearMonthDay(Date);
      return year === y && month === m;
    },
    //判断是否是今天
    isToday (date) {
      let { year, month, day } = utils.getYearMonthDay(new Date());
      let { year: y, month: m, day: d } = utils.getYearMonthDay(date);
      return year === y && month == m && day === d;
    },
    //选择日期输入到文本框
    chooseDate (date) {
      this.time = utils.getYearMonthDay(date); //更新头部年月
      this.$emit('input', date);
      this.blur(); //关闭弹层
    },
    //判断是否选中
    isSelect (date) {
      //获取当前的年月日
      let { year, month, day } = utils.getYearMonthDay(this.value);
      let { year: y, month: m, day: d } = utils.getYearMonthDay(date);
      return year === y && month == m && day === d;
    },
    prevMonth () {
      // this.time.month--;
      // let { year, month, day } = utils.getYearMonthDay(this.time.year, this.time.month, 1); //当前年月
      //获取当前年月的一个日期
      let d = utils.getDate(this.time.year, this.time.month, 1);
      d.setMonth(d.getMonth() - 1);
      this.time = utils.getYearMonthDay(d);

    },
    nextMonth () {
      //获取当前年月的一个日期
      let d = utils.getDate(this.time.year, this.time.month, 1);
      d.setMonth(d.getMonth() + 1);
      this.time = utils.getYearMonthDay(d);
    }
  },
  mounted () {
    console.log(this.visibleDays)
  },
  computed: {
    //日期格式转换
    formatDate () {
      //this.value  //getFullYesr getMonth getDate
      let { year, month, day } = utils.getYearMonthDay(this.value);
      return `${year}-${month + 1}-${day}`
    },
    //
    visibleDays () {
      //先获取当前周几，
      let { year, month } = utils.getYearMonthDay(utils.getDate(this.time.year, this.time.month, 1));
      //获取当前月份的第一天
      let currentFirstDay = utils.getDate(year, month, 1);
      //先生成一个当前日期 2019 9 15
      //获取当前是周几，把天数往前移动几天
      let week = currentFirstDay.getDay();
      //当前开始的天数
      let startDay = currentFirstDay - week * 60 * 60 * 1000 * 24
      console.log(week)
      //把天数往前移动几天 循环42天
      let arr = [];
      for (let i = 0; i < 42; i++) {
        //依次循环出42天
        arr.push(new Date(startDay + i * 60 * 60 * 1000 * 24));
      }
      return arr;
    }
  }
}
</script>
<style scoped lang="stylus">
.pannel {
  width: 32 * 7px;
  // position: absolute;
  background: #fff;
  box-shadow: 2px 2px 2px pink, -2px -2px 2px pink;

  .pannel-nav {
    height: 30px;
    display: flex;
    justify-content: space-around;
    height: 30px;

    span {
      cursor: pointer;
      user-select: none;
    }
  }

  .pannle-content {
    .cell {
      display: inline-flex;
      justify-content: center;
      align-items: center;
      width: 32px;
      height: 32px;
      box-sizing: border-box;
    }

    .cell-days:hover, .select {
      border: 1px solid pink;
      border-radius: 17px;
    }
  }

  .pannel-footer {
    height: 30px;
  }

  .notCurrentMonth {
    color: #dfe1e3;
  }

  .today {
    background: red;
    color: #ffffff;
  }
}
</style>
