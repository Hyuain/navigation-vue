<template>
  <div id="site-list">
    <div class="site" v-for="site in sites" :key="site.url">
      <div class="content" v-longpress="showMobileDelete" @click="openSite(site)">
        <div class="ico">
          <img :src="site.ico" alt="ico" @error="handelIcoError($event)">
          <span class="ico-success">{{site.textIco}}</span>
        </div>
        <div class="name">
          {{site.name}}
        </div>
      </div>
      <div class="mobile-delete-hide">
        <div class="mobile-delete-inner" @click="deleteSite(site)">
          <svg class="icon">
            <use xlink:href="#icon-close"></use>
          </svg>
        </div>
      </div>
      <div class="pc-delete" @click="deleteSite(site)">
        <svg class="icon">
          <use xlink:href="#icon-close"></use>
        </svg>
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
  import Vue from 'vue'
  import LongPress from '../plugins/longpress'

  Vue.use(LongPress, {
    time: 1000
  })

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
        mobileDeleteShow: false
      }
    },
    methods: {
      showMobileDelete(e, el) {
        this.vibration(20)
        el.nextSibling.classList.remove('mobile-delete-hide')
        el.nextSibling.classList.add('mobile-delete-show')
        document.addEventListener('click', () => {
          this.hideMobileDelete(el)
        })
      },
      hideMobileDelete(el) {
        el.nextSibling.classList.add('mobile-delete-hide')
        el.nextSibling.classList.remove('mobile-delete-show')
      },
      vibration(duration) {
        window.navigator.vibrate = window.navigator.vibrate
          || window.navigator.webkitVibrate
          || window.navigator.mozVibrate
          || window.navigator.msVibrate
          || null
        if (window.navigator.vibrate) {
          window.navigator.vibrate(duration)
        }
      },
      addSite() {
        this.eventBus.$emit('openAddForm')
      },
      deleteSite(site) {
        this.sites.splice(this.sites.indexOf(site), 1)
        localStorage.setItem('sites', JSON.stringify(this.sites))
      },
      openSite(site) {
        window.open(site.url, '_self')
      },
      test() {
        localStorage.clear()
        location.reload()
      },
      handelIcoError(event) {
        event.target.classList.add('ico-error')
        event.target.nextSibling.classList.remove('ico-success')
        event.target.nextSibling.classList.add('ico-error')
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
    @media(min-width: 576px) {
      max-width: 460px;
    }
    @media(min-width: 993px) {
      max-width: 900px;
    }
    .site {
      width: 100px;
      height: 100px;
      background-color: white;
      border-radius: 50%;
      box-shadow: 0 1px 1px #b9daff;
      position: relative;
      .content {
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        flex-direction: column;
        align-items: center;
        padding: 10px 0 10px;
        position: relative;
        cursor: pointer;
        .ico {
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
        .name {
          font-size: 14px;
          margin-top: 7px;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          width: 70px;
          text-align: center;
          vertical-align: center;
        }
      }
      .pc-delete {
        display: none;
        @media (min-width: 576px) {
          background-color: #ea4335;
          position: absolute;
          right: 0;
          top: 0;
          cursor: default;
          width: 25px;
          height: 25px;
          border-radius: 50%;
          display: flex;
          justify-content: center;
          align-items: center;
          opacity: 0;
          transition: all .3s;
          .icon {
            color: white;
            width: 15px;
            height: 15px;
          }
          &:hover {
            transform: scale(1.1);
            transition: transform .3s;
            cursor: pointer;
            .icon {
              transform: rotateZ(180deg);
              transition: transform .4s;
            }
          }
        }
      }
      &:hover {
        .pc-delete {
          opacity: 1;
        }
      }
      .mobile-delete-hide {
        display: none;
      }
      .mobile-delete-show {
        background-color: rgba(200, 200, 200, 0.8);
        width: calc(100% + 2px);
        height: calc(100% + 2px);
        position: absolute;
        top: -1px;
        left: -1px;
        border-radius: 4px;
        .mobile-delete-inner {
          background-color: rgba(150, 150, 150, 0.95);
          width: 50px;
          height: 50px;
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translateX(-50%) translateY(-50%);
          border-radius: 50%;
          .icon {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translateX(-50%) translateY(-50%);
            font-size: 30px;
            color: rgba(255, 255, 255, 0.9);
          }
        }
      }
      &.add-button {
        display: flex;
        justify-content: center;
        flex-direction: column;
        align-items: center;
        padding: 10px 0 10px;
        position: relative;
        cursor: pointer;
        .icon {
          width: 50px;
          height: 50px;
          color: #1d78eb;
          transition: transform .4s;
        }
      }
    }
  }
</style>