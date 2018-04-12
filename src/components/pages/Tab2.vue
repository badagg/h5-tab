<template>
  <div class="card-box">
    <div class="row-2" v-for="(item,index) in list" :key="index">
      <Vcard src="http://fakeimg.pl/320x200" title="" type=""/>
    </div>
  </div>
</template>

<script>
  import Vcard from './Vcard'

  export default {
    name: 'Tab2',
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
      }
    }
  }
</script>

<style lang="less">
.card-box {
  padding: .5rem;
  .row-2{
    margin-bottom: 1rem;
  }
}
</style>
