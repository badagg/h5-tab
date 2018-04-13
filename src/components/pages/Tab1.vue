<template>
  <div class="card-box">
    <div class="row-1" v-for="(item,index) in list" :key="index">
      <Vcard v-for="(item,index) in 2" :key="index" src="http://fakeimg.pl/320x200" title="烤面筋哥·辣辣的滋味你尝过没！！" type="鬼畜·明星"/>
    </div>
  </div>
</template>

<script>
  import Vcard from './Vcard'

  export default {
    name: 'Tab1',
    components: {
      Vcard
    },
    data() {
      return {
        list: 0
      }
    },
    methods: {
      initComponent() {
        this.list = 10
      },
      scrollLoad() {
        this.list += 6
      },
      dropDownLoad(loaded) {
        setTimeout(() => {
          console.log('dropDownLoad...')
          loaded()
          this.initComponent()
        },2000)
      }
    }
  }
</script>

<style lang="less">
.card-box {
  padding: .5rem;
  .row-1{
    display: flex;
    margin-bottom: 1rem;
  }
}
</style>
