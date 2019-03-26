<template>
  <!-- 分页 -->
  <div class="paging">
    <button class="prev" @click="prePage" :disabled="nowPage == 1">上一页</button>
    <div class="page-list">
      <button class="page-item" @click="chosePage(1)" :class="{active: nowPage == 1}">1</button>
      <button class="page-item" v-if="ellipsis1">···</button>
      <button class="page-item" v-cloak v-for="(item, index) in pageList" :key="index" :class="{active: item == nowPage}" @click="chosePage(item)">{{item}}</button>
      <button class="page-item" v-if="ellipsis2">···</button>
      <button class="page-item" v-cloak  @click="chosePage(pageLength)" :class="{active: nowPage == pageLength}" v-if="pageLength != 1">{{pageLength}}</button>
    </div>
    <button class="next" @click="nextPage" :disabled="nowPage == pageLength">下一页</button>
  </div>
</template>

<script>
export default {
  //获取父组件传递来的页数和当前页
  props:{
    total:{
      type: Number,
      default: 1,
    },
    pageNum:{
      type: Number,
      default: 1
    },
    pageSize: {
      type: Number,
      default: 10
    }
  },
  data(){
    return{
      ellipsis1: false,
      ellipsis2: false,
    }
  },
  mounted(){
    
  },
  computed:{
    pageLength () {
      return Math.ceil(this.total / 10)
    },
    nowPage: {
      get() {
        return this.pageNum
      },
      set(val) {
        this.$emit("change", val);
      }
    },
    //计算属性，分页要显示的1和最后页中间的页数
    pageList (){
      let list = [];
      if(this.nowPage > 3){ 
        //如果当前页大于3，添加第二页和第三页
        list.push(this.nowPage-2)
        list.push(this.nowPage-1)
        if(this.nowPage > 4){
            //如果当期页大于4，添加。。。
            this.ellipsis1 = true;
        }
        if(this.nowPage == 4){
          //如果当前页=4，隐藏。。。
            this.ellipsis1 = false;
        }
      }else{
        //如果当前页小于3，隐藏。。。,添加第二页
        this.ellipsis1 = false;
        for(let i = 2; i < this.nowPage; i++){
          list.push(i);
        }
      }
      //如果当前页不是第一页也不是最后一页，添加当前页
      if(this.nowPage != 1 && this.nowPage != this.pageLength){
        list.push(this.nowPage);
      }

      if(this.nowPage < (this.pageLength - 2)){
        //如果当前页小于倒数第三页，添加当前页后两页
        list.push(this.nowPage+1);
        list.push(this.nowPage+2)
        if(this.nowPage < (this.pageLength - 3)){
          //如果当前页小于倒数第四页，显示。。。
          this.ellipsis2 = true;
        }
        if(this.nowPage == (this.pageLength - 3)){
          //如果当前页等于倒数第四页，隐藏。。。
          this.ellipsis2 = false;
        }
      }else{
        //如果当前页大于等于倒数第三页，隐藏省略号
        this.ellipsis2 = false;
        //添加当前页到倒数第二页
        for(let i = (this.nowPage+1); i < this.pageLength; i++){
          list.push(i)
        }
      }
      return list;
    }
  },
  methods:{
    chosePage (num){
      if(num != this.nowPage){
        this.nowPage = num
      }
    },
    prePage (){
      this.nowPage --;
    },
    nextPage (){
      this.nowPage ++;
    }
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
