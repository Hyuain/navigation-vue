<template>
  <div id="add-form">
    <input type="text" id="filed-name" placeholder="name" v-model="name">
    <input type="text" id="filed-url" placeholder="url" v-model="url">
    <button class="add" @click="addSite">添加</button>
    <button class="cancel" @click="eventBus.$emit('closeAddForm')">取消</button>
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

</style>