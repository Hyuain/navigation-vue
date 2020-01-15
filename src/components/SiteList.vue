<template>
  <div id="site-list">
    <div class="site" v-for="site in sites" :key="site.url" @click="openSite(site)">
      <div class="ico">
        <img :src="site.ico" alt="ico" @error="handelIcoError($event)">
        <span class="ico-success">{{site.textIco}}</span>
      </div>
      <div class="name">
        {{site.name}}
      </div>
    </div>
    <div class="site add-button" @click="addSite">
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-add"></use>
        </svg>
    </div>
    <button @click="test">清空localstorage</button>
  </div>
</template>

<script>
  import '../assets/icon'
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
      test() {
        localStorage.clear()
        location.reload()
      },
      handelIcoError(event) {
        console.log(event)
        event.target.classList.add('ico-error')
        event.target.nextSibling.classList.remove('ico-success')
        event.target.nextSibling.classList.add('ico-error')
      },
      openSite(site) {
        window.open(site.url, '_self')
      }
    },
    created() {
      if (localStorage.getItem('sites')) {
        this.sites = JSON.parse(localStorage.getItem('sites'))
      }
      this.eventBus.$on('addSite', ({name, url, ico, textIco}) => {
        this.sites.push({name, url, ico, textIco, icoError: false})
        localStorage.setItem('sites', JSON.stringify(this.sites))
      })
    }
  }
</script>

<style lang="scss" scoped>
  #site-list {
    max-width: 100%;
    display: flex;
    flex-wrap: wrap;
    margin-left: auto;
    margin-right: auto;
    @media(min-width: 576px){
      max-width: 460px;
    }
    @media(min-width: 993px){
      max-width: 900px;
    }
    .site {
      width: 100px;
      height: 100px;
      display: flex;
      justify-content: center;
      flex-direction: column;
      align-items: center;
      background-color: white;
      border-radius: 50%;
      padding: 10px 0 10px;
      position: relative;
      cursor: pointer;
      box-shadow: 0 1px 1px #b9daff;
      .ico{
        width: 40px;
        height: 40px;
        display: flex;
        justify-content: center;
        align-items: center;
        text-transform: uppercase;
        font-size: 40px;
        border-radius: 40%;
        overflow: hidden;
        img {
          width: 120%;
          height: 120%;
          &.ico-error {
            display: none;
          }
        }
        span {

          &.ico-error {
            display: inline-block;
          }
          &.ico-success {
            display: none;
          }
        }
      }
      .name{
        font-size: 14px;
        margin-top: 7px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        width: 70px;
        text-align: center;
        vertical-align: center;
      }
      &.add-button{
        .icon{
          width: 50px;
          height: 50px;
          color: #1d78eb;
          transition: transform .4s;
        }
      }
    }
  }
</style>