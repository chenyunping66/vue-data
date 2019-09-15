<!--
 * @Description: In User Settings Edit
 * @Author: your name
 * @Date: 2019-09-14 20:40:19
 * @LastEditTime: 2019-09-15 08:53:59
 * @LastEditors: Please set LastEditors
 -->
<template>
  <div v-click-outside>
    <input type="text" :value="formatDate">
    <div class="pannel" v-if="isVisible">
      context<button>11</button>
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
    return {
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
    }
  },
  computed: {
    //日期格式转换
    formatDate () {
      //this.value  //getFullYesr getMonth getDate
      let { year, month, day } = utils.getYearMonthDay(this.value);
      return `${year}-${month + 1}-${day}`
    }
  }
}
</script>
<style scoped lang="stylus"></style>
