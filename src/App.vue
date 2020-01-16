<template>
  <div>
    <SearchForm id="search-form"></SearchForm>
    <SiteList id="site-list"></SiteList>
    <AddForm id="add-form" v-if="showAddForm"></AddForm>
  </div>
</template>

<script>
  import SearchForm from "./components/SearchForm"
  import SiteList from "./components/SiteList"
  import AddForm from "./components/AddForm"
  import Vue from 'vue'

  export default {
    name: 'App',
    components: {
      SearchForm,
      SiteList,
      AddForm
    },
    data() {
      return {
        eventBus: new Vue({}),
        showAddForm: false
      }
    },
    provide() {
      return {
        eventBus: this.eventBus
      }
    },
    mounted() {
      this.eventBus.$on('openAddForm', () => {
        this.showAddForm = true
      })
      this.eventBus.$on('closeAddForm', () => {
        this.showAddForm = false
      })
    }
  }
</script>

<style lang="scss">
  @import "assets/Reset";
  html,body,#app{
    height: 100%;
  }
  #search-form {
    margin-top: 16%;
  }
  #site-list {
    margin-top: 40px;
    outline: 1px solid red;
  }
  #add-form {
  }
</style>
