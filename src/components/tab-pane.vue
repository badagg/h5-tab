<template>
  <div class="tab-pane" ref="pane" :style="`width:${width}px;height:${height}px;`">
    <slot />
  </div>
</template>

<script>
  export default {
    name: 'TabPane',
    data() {
      return {
        width: 0,
        height: 0,
        loaded: false
      }
    },
    props: {
      position: 0
    },
    watch: {
      '$parent.$parent.p': {
        handler: function(val) {
          if(val === this.position) {
            if(!this.loaded){
              this.initComponent()
            }
          }
        }
      }
    },
    methods: {
      initComponent() {
        if(this.$children && this.$children[0] && typeof this.$children[0].initComponent === 'function') {
          this.$children[0].initComponent()
        }
        this.loaded = true
      },
      scrollLoad() {
        if(this.$children && this.$children[0] && typeof this.$children[0].scrollLoad === 'function') {
          this.$children[0].scrollLoad()
        }
      }
    },
    mounted() {
      this.width = document.body.clientWidth
      this.height = document.body.clientHeight - 41

      if(this.$parent.$parent.p === this.position) {
        this.initComponent()
      }

      let pane = this.$refs.pane
      pane.addEventListener('scroll', () => {
        let sh = pane.scrollHeight - pane.offsetHeight
        if(sh === pane.scrollTop){
          this.scrollLoad()
        }
      })
    }
  }
</script>

<style lang="less">
  .tab-pane {
    position: relative;
    float: left;
    color:#222;
    font-size: 20px;
    background: #fff;
    overflow: auto;
  }
</style>