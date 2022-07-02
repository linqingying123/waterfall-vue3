<template>
  <div>
    <l-waterfall v-if="data" :cols="2" >
            <div class="div" style="height:50px" v-for="(item,index) in data" :key="index">
              {{ item }}
            </div>
    </l-waterfall>
    <div @click="b">添加数据</div>
  </div>

</template>


<script>


// import LWaterfall from "@/components/LWaterfall";

import LWaterfall from "/lib/LWaterfall";
// import LWaterfall from 'LWaterfall'
export default {
  name: 'App',
  components: {
    LWaterfall
  },

  data() {
    return {
      data: [1,2,3,4,5,6,7,8,9,10]
    }
  },
  methods: {
    b() {
      this.data = [...this.data, ...this.data]
      console.log("添加数据", this.data)
    }
  },

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
    border: 5px red solid;
}
/*.div:last-child{*/
/*  border: 5px red solid;*/
/*}*/
</style>
