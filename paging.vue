<template>
  <!-- 分页 -->
  <div class="paging">
    <button class="prev" @click="prePage" :disabled="current == 1">上一页</button>
    <div class="page-list">
      <button class="page-item" @click="chosePage(1)" :class="{active: current == 1}">1</button>
      <button class="page-item" v-if="ellipsis1">···</button>
      <button class="page-item" v-cloak v-for="(item, index) in pageList" :key="index" :class="{active: item == current}" @click="chosePage(item)">{{item}}</button>
      <button class="page-item" v-if="ellipsis2">···</button>
      <button class="page-item" v-cloak  @click="chosePage(pageLength)" :class="{active: current == pageLength}" v-if="pageLength != 1">{{pageLength}}</button>
    </div>
    <button class="next" @click="nextPage" :disabled="current == pageLength">下一页</button>
  </div>
</template>

<script>
export default {
  //获取父组件传递来的页数和当前页
  props:{
    getPageLength:{
      type: Number,
      default: 1,
    },
    getCurrent:{
      type: Number,
      default: 1
    }
  },
  data: function(){
    return{
      ellipsis1: false,
      ellipsis2: false,
      pageLength: this.getPageLength,     //总页数
      current: this.getCurrent            //当前页
    }
  },
  computed:{
    //计算属性，分页要显示的1和最后页中间的页数
    pageList:function(){
      let list = [];
      if(this.current > 3){ 
        //如果当前页大于3，添加第二页和第三页
        list.push(this.current-2)
        list.push(this.current-1)
        if(this.current > 4){
            //如果当期页大于4，添加。。。
            this.ellipsis1 = true;
        }
        if(this.current == 4){
          //如果当前页=4，隐藏。。。
            this.ellipsis1 = false;
        }
      }else{
        //如果当前页小于3，隐藏。。。,添加第二页
        this.ellipsis1 = false;
        for(let i = 2; i < this.current; i++){
          list.push(i);
        }
      }
      //如果当前页不是第一页也不是最后一页，添加当前页
      if(this.current != 1 && this.current != this.pageLength){
        list.push(this.current);
      }

      if(this.current < (this.pageLength - 2)){
        //如果当前页小于倒数第三页，添加当前页后两页
        list.push(this.current+1);
        list.push(this.current+2)
        if(this.current < (this.pageLength - 3)){
          //如果当前页小于倒数第四页，显示。。。
          this.ellipsis2 = true;
        }
        if(this.current == (this.pageLength - 3)){
          //如果当前页等于倒数第四页，隐藏。。。
          this.ellipsis2 = false;
        }
      }else{
        //如果当前页大于等于倒数第三页，隐藏省略号
        this.ellipsis2 = false;
        //添加当前页到倒数第二页
        for(let i = (this.current+1); i < this.pageLength; i++){
          list.push(i)
        }
      }
      return list;
    }
  },
  methods:{
    chosePage: function(num){
      if(num != this.current){
        this.current = num
      }
    },
    prePage: function(){
      this.current --;
    },
    nextPage: function(){
      this.current ++;
    }
  },
  watch: {
    current: function(val){
      this.$emit("sendPage",val);
    },
  }
}
</script>

<style>
  .paging{
    overflow: hidden;
  }
  .paging .page-item{
    float: left;
    width: 40px;
    height: 40px;
    text-align: center;
    line-height: 34px;
    margin: 0 3px;
    cursor: pointer;
    box-sizing: border-box;
  }
  .paging button{
    outline: none;
    float: left;
    width: 70px;
    height: 40px;
    text-align: center;
    line-height: 34px;
    cursor: pointer;
    background: rgba(255, 255, 255, 0.8);
    border-radius: 5px;
    border: none;
    color: #333;
  }
  .paging .prev{
    margin-right: 20px;
  }
  .paging .next{
    margin-left: 20px;
  }
  .page-item.active{
    background: transparent;
    border: 2px solid #ffffff;
    color: #ffffff;
    line-height: 36px;
  }
</style>
