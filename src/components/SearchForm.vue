<template>
  <div id="search-form">
    <form action="https://www.baidu.com/s" method="get">
      <input type="search" v-model="searchContent">
      <button type="submit">search</button>
    </form>
  </div>
</template>

<script>
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

</style>