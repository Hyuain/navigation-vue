<template>
  <div id="site-list">
    <div class="site" v-for="site in sites" :key="site.url">
      <div class="ico">
        <img :src="site.ico" alt="ico" @error="handelIcoError($event)">
        <span class="ico-success">{{site.textIco}}</span>
      </div>
      <div class="name">
        {{site.name}}
      </div>
    </div>
    <div class="site last">
      <div class="add" @click="addSite">
        +
      </div>
    </div>
    <button @click="test">清空localstorage</button>
  </div>
</template>

<script>

  export default {
    name: "SiteList",
    inject: ['eventBus'],
    data() {
      return {
        sites: [
          {
            name: 'OverAPI',
            url: 'http://overapi.com/',
            ico: 'http://overapi.com/static/images/overapi-logo.png',
            textIco: 'O'
          },
          {
            name: 'MDN',
            url: 'https://developer.mozilla.org/zh-CN/',
            ico: 'https://developer.mozilla.org/favicon.ico',
            textIco: 'M'
          }
        ],
      }
    },
    methods: {
      addSite() {
        this.eventBus.$emit('openAddForm')
        console.log('run')
      },
      test(){
        localStorage.clear()
        location.reload()
      },
      handelIcoError(event){
        console.log(event)
        event.target.classList.add('ico-error')
        event.target.nextSibling.classList.remove('ico-success')
        event.target.nextSibling.classList.add('ico-error')
      }
    },
    created() {
      if(localStorage.getItem('sites')){
        this.sites = JSON.parse(localStorage.getItem('sites'))
      }
      this.eventBus.$on('addSite', ({name, url, ico ,textIco})=>{
        this.sites.push({name, url, ico, textIco, icoError: false})
        localStorage.setItem('sites', JSON.stringify(this.sites))
      })
    }
  }
</script>

<style lang="scss" scoped>
  .site {
    border: 1px solid red;
  }
  img{
    &.ico-error{
      display: none;
    }
  }
  span{
    &.ico-error{
      display: inline-block;
    }
    &.ico-success{
      display: none;
    }
  }
</style>