<template>
  <div class="tabs">
    <div class="tab-nav">
      <ul>
        <li v-for="(item, index) in list" :key="index" v-text="item.name" @click="onTap(index)"></li>
        <i class="line" ref="line" :style="`width:${100 / len}%; transform: translateX(${lineX}px);`"></i>
      </ul>
    </div>
    <div class="tab-content">
      <v-touch v-on:panmove="move" v-on:panend="end" v-bind:pan-options="{ direction: 'horizontal', threshold: 45 }">
        <div class="pane-box" ref="pane" :style="`transform: translateX(${dx}px)`">
          <slot></slot>
        </div>
      </v-touch>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Tabs',
    props: {
      list: {
        type: Array,
        default: () => {
          return []
        }
      },
      page: {
        type: Number,
        default: 0
      }
    },
    data() {
      return {
        width: 0,
        line: null,
        lineW: 0,
        p: this.page - 1,
        dx: 0,
        pane: null
      }
    },
    computed: {
      len() {
        return this.list.length
      },
      lineX() {
        return -this.dx * this.lineW / this.width
      }
    },
    mounted() {
      this.pane = this.$refs.pane
      this.line = this.$refs.line
      this.width = document.body.clientWidth
      this.lineW = this.line.offsetWidth
      
      this.dx = -this.p * this.width
    },
    methods: {
      move(e) {
        this.pane.style.transition = 'none'
        this.line.style.transition = 'none'
        this.dx = e.deltaX + (-this.p * this.width)
      },
      end(e) {
        if(Math.abs(e.deltaX) > this.width / 3 || e.deltaTime < 135){
          if(e.deltaX > 0) {
            if(this.p > 0) {
              this.p--
            }
          }else{
            if(this.p < this.len-1){
              this.p++
            }
          }
        }
        this.change()
      },
      change() {
        this.pane.style.transition = 'transform .3s'
        this.line.style.transition = 'transform .3s'
        this.dx = -this.p * this.width
      },
      onTap(index) {
        this.p = index
        this.change()
      }
    }
  }
</script>

<style lang="less">
  .tabs{
    background-color: #222;
    background-image: url(../assets/bili-log.png);
    background-position: center 100px;
    background-repeat: no-repeat;
  }
  .tab-nav {
    border-bottom: 1px solid #f1f1f1;
    background-color: #fff;
    ul{
      display: flex;
      li {
        flex: 1;
        height: 40px;
        line-height: 40px;
      }
      .line {
        position: absolute;
        top: 39px;
        height: 2px;
        background-color: #f60;
      }
    }
  }
  .tab-content{
    width:100%;
    overflow: hidden;
  }
  .pane-box {
    width: 9999%;
  }
</style>