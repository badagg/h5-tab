<template>
  <div class="v-card">
    <div class="pic">
      <img :src="src">
    </div>
    <div class="title" v-text="title"></div>
    <div class="type" v-text="type"></div>
  </div>
</template>

<script>
  export default {
    props: ['title', 'src', 'type']
  }
</script>

<style lang="less">
.v-card {
  flex: 1;
  text-align: left;
  font-size: .8rem;
  padding: .2rem;
  overflow: hidden;
  .pic{
    text-align: center;
    height: 7rem;
    img{
      width: 100%;
      border-radius: 4px;
    }
  }
  .title{
    padding-top: 5px;
    height: 40px;
    line-height: 20px;
  }
  .type{
    height: 20px;
    line-height: 20px;
  }
}
</style>
