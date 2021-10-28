<template>
  <div class="hello">
    <div class="title">最短路径</div>
    <div  v-for="(item,index) in arr" :key="index">
      <span
          :class="{'barrier': subItem==='barrier','target': subItem==='target'}"
          v-for="(subItem,subIndex) in item" :key="subIndex"></span>
    </div>
  </div>
</template>

<script>
import {toRefs,onMounted}  from  'vue'
export default {
  name: 'Main',
  props: {
    msg: String
  },
  setup(){
    let data = {
      row: 10,
      col: 10,
      arr: [],
    }
    // 初始化矩阵
    for(let i=0;i<data.row;i++){
      data.arr.push([])
      for(let j=0;j<data.col;j++){
        if(i>=2 && i<=8 && j===3) {
          data.arr[i][j] = 'barrier'
        }else if(i===2 && j>3 &&j<8){
          data.arr[i][j] = 'barrier'
        }else if(i===5 && j===6){
          data.arr[i][j] = 'target'
        } else {
          data.arr[i][j] = false
        }
      }
    }
    // 设置障碍
    console.log(data.arr)
    onMounted(()=>{

    })
    return {
      ...toRefs(data),
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped >
.title{
  color: white;
}
div{

}
span{
  width: 20px;
  height: 20px;
  background: white;
  border: 1px solid red;
  display: inline-block;
}
.barrier{
  background: red;
}
.target{
  background: green;
}
</style>
