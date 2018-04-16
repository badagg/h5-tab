<template>
  <div class="tabs">
    <div class="tab-nav">
      <ul>
        <li v-for="(item, index) in list" :key="index" v-text="item.name" @click="onTap(index)"></li>
        <i class="line" ref="line" :style="`width:${100 / len}%; transform: translate3d(${lineX}px, 0, 0);`"></i>
      </ul>
    </div>
    <div class="tab-content">
      <v-touch v-on:panmove="move" v-on:panend="end" v-bind:pan-options="{ threshold: 45 }">
        <div class="pane-box" ref="pane" :style="`transform: translate3d(${dx}px, 0, 0);`">
          <slot></slot>
        </div>
      </v-touch>
    </div>
    <div ref="loading" class="loading" :style="`transform: translate3d(0, ${loadingY}px, 0) rotate3d(0,0,1,${loadingR}deg)`">
      <i :class="['vanfont van-icon-loading', loadingY === loadingTop ? 'van-icon-rotate' : '' ]"></i>
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
        pane: null,
        loadingY: 0,
        loadingR: 0,
        loadingTop: 120
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
      this.loading = this.$refs.loading
      this.width = document.body.clientWidth
      this.lineW = this.line.offsetWidth
      
      this.dx = -this.p * this.width
    },
    methods: {
      move(e) {
        this.pane.style.transition = 'none'
        this.line.style.transition = 'none'
        this.loading.style.transition = 'transform .1s'
        
        let x = e.deltaX
        if(Math.abs(e.deltaX) > Math.abs(e.deltaY)) {
          if(this.p === 0) {
            x = e.deltaX > 0 ? 0 : e.deltaX
          }
          if(this.p === this.len -1) {
            x = e.deltaX < 0 ? 0 : e.deltaX
          }

          this.dx = x + (-this.p * this.width)
        }else {
          if(this.$children[0].$children[this.p].scrollTop > 0) return
          if(e.deltaY < this.loadingTop) {
            this.loadingY = e.deltaY
            this.loadingR = 360 * e.deltaY / this.loadingTop
          }else {
            this.loadingY = this.loadingTop
            this.loadingR = 360
          }
        }

        this.$children[0].$children[this.p].pauseScroll()
      },
      end(e) {
        if(Math.abs(e.deltaX) > Math.abs(e.deltaY)) {
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
        }else {
          this.loading.style.transition = 'transform .3s'
          if(e.deltaY < this.loadingTop) {
            this.loadingY = 0
            this.loadingR = 0
          }else {
            this.dropDownLoad()
          }
        }

        this.change()
        this.$children[0].$children[this.p].startScroll()
      },
      change() {
        this.pane.style.transition = 'transform .3s'
        this.line.style.transition = 'transform .3s'
        this.dx = -this.p * this.width
      },
      onTap(index) {
        this.p = index
        this.change()
      },
      dropDownLoad() {
        this.$children[0].$children[this.p].dropDownLoad(() => {
          this.loading.style.transition = 'transform .3s'
          this.loadingY = 0
          this.loadingR = 0
        })
      }
    }
  }
</script>

<style lang="less">
  @keyframes loadingCircle {
    0% {
      transform-origin: 50% 50%;
      transform: rotate(0deg);
    }
    100% {
      transform-origin: 50% 50%;
      transform: rotate(360deg);
    }
  }
  .tabs{
    background-color: #222;
    background-image: url(../assets/bili-log.png);
    background-position: center 100px;
    background-repeat: no-repeat;
    overflow: hidden;
    position: relative;
    .tab-nav {
      position: relative;
      z-index: 10;
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
      position: relative;
      z-index: 1;
      width:100%;
      overflow: hidden;
    }
    .pane-box {
      width: 9999%;
    }

    .van-icon-rotate:before {
      display: inline-block;
      animation: loadingCircle 1s infinite linear;
    }

    .loading {
      position: absolute;
      z-index: 9;
      width: 28px;
      height: 28px;
      background: #fff;
      left: 50%;
      top: 0;
      margin-left: -15px;
      border-radius: 50%;
      box-shadow: 0 0 5px rgba(0,0,0,.2);
      .vanfont {
        color: #f60;
        font-size: 26px;
      }
    }
  }
  
</style>