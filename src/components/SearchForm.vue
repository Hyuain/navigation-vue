<template>
  <div id="search-form">
    <form action="https://www.baidu.com/s" method="get">
      <input type="search" v-model="searchContent">
      <button type="submit">
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-search"></use>
        </svg>
      </button>
    </form>
  </div>
</template>

<script>
  import '../assets/icon'
  export default {
    name: "SearchForm",
    data(){
      return{
        searchContent: ''
      }
    }
  }
</script>

<style lang="scss" scoped>
  #search-form{
    form{
      display: flex;
      max-width: 500px;
      margin-left: auto;
      margin-right: auto;
      padding: 0 10px;
      border-radius: 20px;
      background: white;
      box-shadow: 0 1px 3px #bbb;
      input{
        flex-grow: 1;
        height: 40px;
        margin-left: 10px;
        margin-right: 10px;
        border: none;
        background: none;
        font-size: 16px;
        &:focus{
          outline: none;
        }
      }
      button{
        flex-shrink: 0;
        border: none;
        background: none;
        padding-right: 7px;
        color: #1d78eb;
        font-size: 20px;
      }
    }
  }
</style>