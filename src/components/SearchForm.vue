<template>
  <div id="search-form">
    <div class="logo">
      <transition name="slide-fade">
        <img class="google" src="../../static/images/google-white.png" alt="google-logo" v-if="searchEngine === 'google'">
      </transition>
      <transition name="slide-fade">
        <img class="baidu" src="../../static/images/baidu-white.png" alt="baidu-logo" v-if="searchEngine === 'baidu'">
      </transition>
      <transition name="slide-fade">
        <img class="bing" src="../../static/images/bing-white.png" alt="bing-logo" v-if="searchEngine === 'bing'">
      </transition>
    </div>
    <form :action="requestUrl[this.searchEngine].path" method="get">
      <div class="search-engine">
        <svg class="icon google active" @click="changeSearchEngine($event,'google')">
          <use xlink:href="#icon-google"></use>
        </svg>
        <svg class="icon baidu" @click="changeSearchEngine($event,'baidu')">
          <use xlink:href="#icon-baidu"></use>
        </svg>
        <svg class="icon bing" @click="changeSearchEngine($event,'bing')">
          <use xlink:href="#icon-bing"></use>
        </svg>
      </div>
      <input type="search" v-model="searchContent" placeholder="想要找些什么呢？" :name="requestUrl[this.searchEngine].query" >
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
    data() {
      return {
        searchContent: '',
        searchEngine: 'google',
        timer: null,
        requestUrl: {
          google:{
            path: 'https://www.google.com/search',
            query: 'q'
          },
          baidu:{
            path: 'https://www.baidu.com/s',
            query: 'wd'
          },
          bing:{
            path: 'https://cn.bing.com/search',
            query: 'q'
          },
          '':{
            path:'',
            query:''
          }
        }
      }
    },
    methods: {
      changeSearchEngine(e, value) {
        clearTimeout(this.timer)
        this.searchEngine = ''
        e.currentTarget.parentNode.childNodes.forEach((el) => {
          el.classList.remove('active')
        })
        e.currentTarget.classList.add('active')
        this.timer = setTimeout(() => {
          this.searchEngine = value
        }, 400)
      }
    }
  }
</script>

<style lang="scss" scoped>
  #search-form {
    max-width: 90%;
    margin-left: auto;
    margin-right: auto;
    position: relative;
    @media(min-width: 576px) {
      max-width: 560px;
    }
    .logo {
      width: 100%;
      position: relative;
      .slide-fade-enter-active {
        transition: all .3s ease;
      }
      .slide-fade-leave-active {
        transition: all .3s ease;
      }
      .slide-fade-enter, .slide-fade-leave-to {
        transform: translateY(10px);
        opacity: 0;
      }
      img {
        &.google {
          position: absolute;
          width: 240px;
          top: -82px;
          left: 50%;
          margin-left: -124px;
          @media (min-width: 576px) {
            width: 300px;
            top: -102px;
            left: 0;
            margin-left: 0;
          }
        }
        &.baidu {
          position: absolute;
          width: 250px;
          top: -84px;
          left: 50%;
          margin-left: -124px;
          @media (min-width: 576px) {
            width: 312px;
            top: -106px;
            left: 4px;
            margin-left: 0;
          }
        }
        &.bing {
          position: absolute;
          width: 206px;
          top: -82px;
          left: 50%;
          margin-left: -110px;
          @media (min-width: 576px) {
            width: 258px;
            top: -102px;
            left: 4px;
            margin-left: 0;
          }
        }
      }
    }
    form {
      margin-left: auto;
      margin-right: auto;
      display: flex;
      padding: 0 10px;
      border-radius: 26px;
      background: white;
      box-shadow: 0 1px 2px #aaa;
      align-items: center;
      .search-engine {
        flex-shrink: 0;
        .icon {
          font-size: 24px;
          margin-right: 3px;
          position: relative;
          z-index: 1;
          transition: all .2s;
          color: #777;
          &.baidu{
            font-size: 25px;
          }
          &:hover{
            transform: translateY(-2px);
            cursor: pointer;
          }
          &.active,&:hover {
            &.google {
              color: #e63f2d;
            }
            &.baidu {
              color: #233fe4;
            }
            &.bing {
              color: #fdb600
            }
          }
        }
      }
      input {
        flex-grow: 1;
        min-width: 0;
        height: 48px;
        margin-left: 5px;
        margin-right: 10px;
        background: none;
        font-size: 18px;
        border: none;
        position: relative;
        z-index: 1;
        &:focus {
          outline: none;
        }
      }
      button {
        flex-shrink: 0;
        border: none;
        background: none;
        padding-right: 7px;
        color: #1d78eb;
        font-size: 24px;
      }
    }
  }
</style>