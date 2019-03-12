<template>
  <div class="pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button v-if="judge" class="pageBtn">...</button>
    <button v-for="btn in pageBtn" :key="btn"
    :class="[{currentPage: btn === currentPage},'pageBtn']"
    @click="changeBtn(btn)">
      {{btn}}
    </button>
    <button class="pageBtn">...</button>
    <button @click="changeBtn">下一页</button>
  </div>
</template>

<script>
import $ from 'jquery'
export default {
  name: 'Pagination',
  data () {
    return {
      pageBtn: [1,2,3,4,5],
      currentPage: 1,
      judge: false
    }
  },
  methods: {
    changeBtn(page){
      if(typeof page != 'number'){
        switch(page.target.innerText){
          case '上一页':
          $('button.currentPage').prev().click()
          break;
          case '下一页':
          $('button.currentPage').next().click()
          break;
          case '首页':
          this.pageBtn = [1,2,3,4,5]
          this.currentPage =1
          break;
          default:
          break;
        }
        return
      }
      this.currentPage = page
      if(page === this.pageBtn[4]){
        this.pageBtn.shift()
        this.pageBtn.push(this.pageBtn[3]+1)
        this.judge = true
      }
      if(page === this.pageBtn[0] && page != 1){
        this.pageBtn.unshift(this.pageBtn[0]-1)
        this.pageBtn.splice(5,1)
      }
      if(page <3) {
        this.judge = false
      }

      this.$emit('handleList',this.currentPage)
    }
  }
}
</script>

<style scoped>
   .pagination {
    border: 1px solid #ccc;
    padding: 6px 20px;
    margin: 10px;
    border-radius: 4px;
  }

  .pagination button {
    margin-right: 6px;
    border-style: none;
    background-color: #fff;
    padding: 6px;
    border: 1px solid #ccc;
    border-radius: 3px;
    outline: none;
    cursor: pointer;
    transition: all .3s;
  }

   .pageBtn {
    width: 40px;
  }

  .pagination .currentPage {
    color: white;
    border: 1px solid #3599f7;
    background-color: #3599f7;
  }
</style>
