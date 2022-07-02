<template>
  <div id="waterfall" class="waterfall" ref="waterfall">
    <slot>
    </slot>
  </div>
</template>

<script>
import {onMounted, onUpdated, ref, toRefs} from 'vue'


export default {
  name: "LWaterfall",
  props: {
    cols: {
      type: Number,
      default: 2
    },
    colWidth: {
      type: Number,
      default: 0
    },
    data: {
      type: Array,
      default: null
    }


  },

  // watch: {
  //   data(newData, oldData) {
  //     console.log('数据发生变化')
  //     console.log('newdata', newData)
  //     console.log('olddata', oldData)
  //   }
  // },

  setup: function (props, context) {
    console.log(context)
    const {cols, colWidth} = toRefs(props)
    let col = 0
    let isWidth = false
    // let children = null
    console.log(cols.value, colWidth.value)


    let divs = {}

    function init(waterfallel) {
      divs = {}
      console.log(waterfallel)
      console.log('初始化')

      // //计算列宽
      console.log('计算列数')
      if (colWidth.value == null || colWidth.value == undefined || colWidth.value == "") {

        col = cols.value
        console.log("使用默认列宽")
      } else {
        isWidth = true
        //获取窗口宽度
        let windowWidth = window.innerWidth;
        console.log(windowWidth)
        //计算列数
        col = Math.floor(windowWidth / colWidth.value);
        console.log("使用计算列宽")
      }
      console.log("列数", col)

      //创建dom
      console.log('创建dom')


      for (let i = 0; i < col; i++) {
        let item = document.createElement("div")
        item.className = 'item'
        if (isWidth) {
          item.style.width = colWidth.value + "px"
        } else {
          item.style.flex = 1
        }

        // item.classList.add('item')
        divs[i] = {
          height: 0,
          div: item
        }

      }
      console.log(divs)


      // //处理dom应该在挂载时
      console.log('处理dom')
      let children = waterfallel.value.children
      // waterfallel.value.innerHTML = ''
      console.log(waterfallel.value.children.length)
      while (children.length > 0) {

        let child = children[0]
        console.log('child', child)
        child.style.width = 'auto'
        let minIndex = findMinHeight()
        divs[minIndex].height += child.clientHeight
        divs[minIndex].div.appendChild(child)
      }

      //  添加到dom中
      console.log("添加到dom")
      for (let i = 0; i < col; i++) {
        waterfallel.value.appendChild(divs[i].div)
      }


    }


    //寻找最短列
    function findMinHeight() {
      let minHeight = 99999999;
      let minIndex = 0;
      for (let i = 0; i < col; i++) {

        let item = divs[i]
        if (item.height < minHeight) {
          minHeight = item.height
          minIndex = i
        }
      }
      return minIndex
    }


    const waterfall = ref(null)
    console.log(waterfall)
    // onBeforeMount(()=>{
    //   console.log(waterfall.value.children)
    // })
    // onBeforeUpdate(()=>{
    //   console.log(waterfall.value.children)
    // })

    onUpdated(() => {
      console.log('更新')
      console.log(waterfall.value.children.length)
      while (waterfall.value.children.length > 0) {
        let child = waterfall.value.children[0]
        console.log(child)
        if (child.className == 'item') {
          console.log('删除')
          waterfall.value.removeChild(child)
        } else {
          console.log('添加')
          child.style.width = 'auto'
          let minIndex = findMinHeight()
          divs[minIndex].height += child.clientHeight
          divs[minIndex].div.appendChild(child)
        }
      }

      for (let i = 0; i < col; i++) {
        waterfall.value.appendChild(divs[i].div)
      }
    })
    onMounted(() => {
      // watch(data, (newVal, oldVal) => {
      //   console.log('数据发生变化')
      //   console.log(waterfall.value.children.length)
      //
      //   // init(waterfall)
      //   // console.log(waterfall)
      //
      //
      //
      //   console.log('newdata', newVal)
      //   console.log('olddata', oldVal)
      // })

      init(waterfall)


    })


    return {
      waterfall,
      init,


    }
  },


}
</script>

<style scoped>
.waterfall {
  display: flex;
  justify-content: center;
}

/*.waterfall > div.item {*/
/*  flex: 1;*/
/*}*/


*, body {
  box-sizing: border-box;
}
</style>
