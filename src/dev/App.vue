<template>
  <div>


    <l-waterfall v-if="data" :cols="2" >
      <div class="div" v-for="(item, index) in data"
           :key="index">

        <div style="margin:30px 0">

          <img :src="item.coverImg" style="width:100%;height:100%"/>
          <div>{{ item.title }}</div>
        </div>
      </div>
      <!--      <div class="div" style="height:50px" v-for="(item,index) in data" :key="index">-->
      <!--        {{ item.author }}-->
      <!--      </div>-->

    </l-waterfall>
    <div @click="b">添加数据</div>

  </div>

</template>


<script>


import LWaterfall from "@/components/LWaterfall";
import axios from "axios";

export default {
  name: 'App',
  components: {
    LWaterfall

  },

  data() {
    return {
      is: false,
      data: null
    }
  },



  methods: {
    b() {

      this.data = [...this.data, ...this.data]
      console.log("添加数据", this.data)
    }
  },
  created() {
    axios.get('http://lncc.linqingying.xyz/jeecg-boot/lncc/api/all').then(res => {
      console.log(res.data.result.newsList.records)
      this.is = true
      console.log('显示')
      this.data = res.data.result.newsList.records
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.div{
  margin:30px;
}
/*.div:last-child{*/
/*  border: 5px red solid;*/
/*}*/
</style>
