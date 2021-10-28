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
          data.arr[i][j] = 'no'
        }
      }
    }
    function dfs(row,col){
      if(row<0 || col<0 || row>data.row-1 || col>data.col-1 || data.arr[row][col] === 'barrier' || data.arr[row][col] === 'yes'){
        return false
      }
      if(data.arr[row][col] === 'target'){
        return 1
      }
      if(data.arr[row][col] === 'no'){
        data.arr[row][col] = 'yes'
        let a = dfs(row,col+1)
        let b = dfs(row+1,col)
        let c = dfs(row,col-1)
        let d = dfs(row-1,col+1)
        if(!a && !b && !c && !d) return false
        let min = a || b || c || d
        if(a){if(a<min) min=a}
        if(b){if(b<min) min=b}
        if(c){if(c<min) min=c}
        if(d){if(d<min) min=d}
        return min + 1
      }
    }
    let s = dfs(0,0)
    // 设置障碍
    console.log(data.arr,s)
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
