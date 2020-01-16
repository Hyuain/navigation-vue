<template>
  <div id="add-form">
    <div id="add-form-content">
      <div class="title">添加网站</div>
      <div class="form-item name">
        <label for="name">名称</label>
        <input type="text" id="name" v-model="name">
      </div>
      <div class="form-item url">
        <label for="url">网址</label>
        <input type="text" id="url" v-model="url">
      </div>
      <div class="buttons">
        <button class="add" @click="addSite">添加</button>
        <button class="cancel" @click="eventBus.$emit('closeAddForm')">取消</button>
      </div>
    </div>

  </div>
</template>

<script>
  export default {
    name: "AddForm",
    inject: ['eventBus'],
    data() {
      return {
        name: '',
        url: '',
        ico: '',
        textIco: ''
      }
    },
    created() {
      this.eventBus.$on('openAddForm', () => {
        console.log('openAddForm')
      })
    },
    methods: {
      addSite() {
        if (!this.name) {
          this.name = this.simplifyUrl(this.url)
        }
        if (!this.url) {
          alert('请务必输入网址哦')
          return
        }
        this.ico = 'https://' + this.simplifyUrl(this.url) + '/favicon.ico'
        if (this.url.indexOf('http') < 0) {
          this.url = 'https://' + this.url
        }
        this.textIco = this.name[0].toUpperCase()
        this.eventBus.$emit('addSite', {name: this.name, site: this.url, ico: this.ico, textIco: this.textIco})
        this.eventBus.$emit('closeAddForm')
      },
      simplifyUrl(url) {
        return url.replace('https://', '')
          .replace('http://', '')
          .replace('www.', '')
          .replace(/\/.*/, '')
      }
    }
  }
</script>

<style lang="scss" scoped>
  #add-form {
    position: fixed;
    z-index: 2;
    height: 100vh;
    width: 100vw;
    left: 0;
    top: 0;
    #add-form-content {
      position: absolute;
      background: white;
      box-shadow: 0 1px 3px #ccc;
      border-radius: 7px;
      top: 50%;
      left: 50%;
      transform: translateX(-50%) translateY(-50%);
      padding: 5px;
      .title {
        margin: 15px;
      }
      .form-item {
        margin: 15px;
        width: 270px;
        label {
          font-size: 15px;
          color: #555;
          @media(max-width: 576px) {
            font-size: 12px;
          }
        }
        input {
          padding: 7px;
          width: 100%;
          border-radius: 4px;
          border: none;
          background: #f1f3f4;
          font-size: 16px;
          box-shadow: inset 0 0 1px #f1f3f4;
          transition: box-shadow .2s;
          &:focus {
            box-shadow: inset 0 0 2px #1d78eb;
            outline: none;
          }
        }
        &.name {}
        &.url {}
      }
      .buttons {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        margin: 20px auto 10px;
        button.add {
          width: 270px;
          height: 35px;
          margin-bottom: 10px;
          border-radius: 4px;
          background-color: #1d78eb;
          border: none;
          color: white;
          font-size: 16px;
          &:hover{
            background-color: #2c80ea;
            cursor: pointer;
          }
        }
        button.cancel {
          width: 270px;
          height: 35px;
          border-radius: 4px;
          background-color: white;
          border: 1px solid #ccc;
          color: #e63f2d;
          font-size: 16px;
          &:hover{
            background-color: #f8faff;
            cursor: pointer;
          }
        }
      }
    }
  }
</style>