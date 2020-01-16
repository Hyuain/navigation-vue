<template>
  <div id="site-list">
    <div class="site-list-inner">
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
          <div class="mobile-delete-bg">
            <div class="mobile-delete-inner" @click="deleteSite(site)">
              <svg class="icon">
                <use xlink:href="#icon-close"></use>
              </svg>
            </div>
          </div>
        </div>
        <div class="pc-delete" @click="deleteSite(site)">
          <svg class="icon">
            <use xlink:href="#icon-close"></use>
          </svg>
        </div>

      </div>
      <div class="site add-button" @click="addSite" :class="{'active': eventBus.addFormOpen}">
        <svg class="icon" aria-hidden="true" :class="{'active': eventBus.addFormOpen}">
          <use xlink:href="#icon-add"></use>
        </svg>
      </div>
    </div>
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
          },
          {
            name: 'Validator',
            url: 'http://validator.w3.org/',
            ico: 'http://w3.org/favicon.ico',
            textIco: 'V'
          },
          {
            name: 'iconfont',
            url: 'https://www.iconfont.cn/',
            ico: 'https://imgs.91sotu.com/file/icon/qEPydkxQpVjAprM8QuaQptr1.png',
            textIco: 'I'
          },
          {
            name: 'BootCDN',
            url: 'https://www.bootcdn.cn/',
            ico: 'https://www.bootcdn.cn/assets/img/bootcdn.png',
            textIco: 'B'
          },
          {
            name: 'Bilibili',
            url: 'https://www.bilibili.com/',
            ico: 'https://www.bilibili.com/favicon.ico',
            textIco: 'B'
          },
        ],
        mobileDeleteShow: false,

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
  @keyframes mobile-delete-bg-show {
    0% {width: 0;height: 0;}
    100% {width: 142%;height: 142%;}
  }
  @keyframes mobile-delete-inner-show {
    0% {width: 0;height: 0;font-size: 0;}
    100% {width: 50px;height: 50px;font-size: 30px;}
  }
  #site-list {
    margin-left: auto;
    margin-right: auto;
    max-width: 100%;
    @media(min-width: 576px) {
      max-width: 460px;
    }
    @media(min-width: 993px) {
      max-width: 940px;
    }
    .site-list-inner {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-evenly;
      @media(min-width: 576px) {
        margin-right: -20px;
        justify-content: flex-start;
      }
      .site {
        width: 100px;
        height: 100px;
        margin-left: 7px;margin-right: 7px;margin-bottom: 20px;
        background-color: white;
        border-radius: 50%;
        box-shadow: 0 1px 1px #b9daff;
        position: relative;
        transition: all .2s;
        @media(min-width: 576px) {
          margin-left: 0;
          margin-right: 20px;
        }
        &:hover, &:active {
          box-shadow: 0 1px 3px #b9daff;
          color: #1d78eb;
          .pc-delete {
            opacity: 1;
          }
        }
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
            border-radius: 30%;
            overflow: hidden;
            img {
              width: 100%;
              height: 100%;
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
                transition: transform .5s;
              }
            }
          }
        }
        .mobile-delete-hide {
          display: none;
        }
        .mobile-delete-show {
          width: calc(100% + 2px);
          height: calc(100% + 2px);
          position: absolute;
          top: -1px;
          left: -1px;
          border-radius: 16px;
          overflow: hidden;
          .mobile-delete-bg {
            background-color: rgba(225, 225, 225, 0.8);
            animation: mobile-delete-bg-show .3s forwards;
            border-radius: 50%;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translateX(-50%) translateY(-50%);
            .mobile-delete-inner {
              animation: mobile-delete-inner-show .4s forwards;
              background-color: rgba(255, 62, 63, 0.7);
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
                color: rgba(255, 255, 255, 0.9);
              }
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
          transition: all .3s;
          cursor: pointer;
          box-shadow: 0 1px 1px #b9daff;
          &:hover, &.active {
            background-color: #1d78eb;
            box-shadow: 0 1px 3px #b9daff;
            cursor: pointer;
            .icon {
              color: white;
              transform: rotateZ(90deg);
              transition: all .3s;
            }
          }
          .icon {
            width: 50px;
            height: 50px;
            color: #1d78eb;
            transition: transform .4s;
          }
        }
      }
    }
  }
</style>