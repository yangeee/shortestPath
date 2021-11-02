<template>
  <div class="hello">
    <div class="title">最短路径 {{ count }}</div>
    <button @click="bfs">计算</button>
    <div  v-for="(item,index) in arr" :key="index">
      <span
          :class="{'barrier': subItem==='barrier','target': subItem==='target','yes':subItem==='yes'}"
          v-for="(subItem,subIndex) in item" :key="subIndex"></span>
    </div>
  </div>
</template>

<script>
import {toRefs,onMounted,reactive}  from  'vue'
export default {
  name: 'Main',
  props: {
    msg: String
  },
  setup(){
    let data = reactive({
      row: 10,
      col: 10,
      arr: [],
      count: 1,
    })
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
    function bfs(){
      let queue = [[0,0]], count = 1
      while(queue.length>0){
        let size = queue.length
        while(size>0){
          let [row,col] = queue.shift()
          if(data.arr[row][col] === 'target') {
            data.count = count
            return
          }
          if(data.arr[row][col] === 'barrier') {
            size--
            continue
          }
          if(data.arr[row][col] === 'yes'){
            size--
            continue
          }
          if(row>=0 && row<data.row && col+1>=0 && col+1<data.col){
            queue.push([row,col+1])
          }
          if(row+1>=0 && col>=0 && col<data.col && row+1<data.row){
            queue.push([row+1,col])
          }
          if(row-1>=0 && col>=0 && col<data.col){
            queue.push([row-1,col])
          }
          if(row>=0 && col-1>=0 && row <data.row){
            queue.push([row,col-1])
          }
          data.arr[row][col] = 'yes'
          size --
        }
        count++
      }
    }
    // let min = 0,count = 1
    // function dfs(row,col,count){
    //   if(row<0 || col<0 || row>data.row-1 || col>data.col-1 || data.arr[row][col] === 'barrier' || data.arr[row][col] === 'yes'){
    //     return false
    //   }
    //   if(data.arr[row][col] === 'target'){
    //     if(min === 0){
    //       min = count
    //     }else if(min > count){
    //       min = count
    //     }
    //     return true
    //   }
    //   if(data.arr[row][col] === 'no'){
    //     data.arr[row][col] = 'yes'
    //     dfs(row,col+1,count+1)
    //     dfs(row+1,col,count+1)
    //     dfs(row,col-1,count+1)
    //     dfs(row-1,col+1,count+1)
        // let a = dfs(row,col+1)
        // let b = dfs(row+1,col)
        // let c = dfs(row,col-1)
        // let d = dfs(row-1,col+1)
        // if(!a && !b && !c && !d) return false
        // let min = a || b || c || d
        // if(a){if(a<min) min=a}
        // if(b){if(b<min) min=b}
        // if(c){if(c<min) min=c}
        // if(d){if(d<min) min=d}
        // return min + 1
    //   }
    // }
    //   dfs(0,0,count)

    // 设置障碍
    console.log(data.arr)
    onMounted(()=>{
    })
    return {
      ...toRefs(data),
      bfs
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
.yes{
  background: yellow;
}
</style>
