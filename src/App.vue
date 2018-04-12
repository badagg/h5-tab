<template>
  <div id="app">
    <tabs :list="list" :page="1">
      <tab-pane v-for="(item, index) in list" :key="index" :position="index">
        <component :is="item.component" />
      </tab-pane>
    </tabs>
  </div>
</template>

<script>
import Tabs from './components/tabs'
import TabPane from './components/tab-pane'

import Live from './components/pages/Tab1'
import Bangumi from './components/pages/Tab1'
import Home from './components/pages/Tab2'
import Cinephile from './components/pages/Tab1'
import Read from './components/pages/Tab2'

export default {
  name: 'App',
  components: {
    Tabs,
    TabPane,
    Live, Home, Bangumi, Cinephile, Read
  },
  data() {
    return {
      list: [
        { name: '直播', component: 'Live' },
        { name: '推荐', component: 'Home' },
        { name: '追番', component: 'Bangumi' },
        { name: '影视', component: 'Cinephile' },
        { name: '专栏', component: 'Read' }
      ]
    }
  }
}
</script>

<style lang="less">
html,body{
  height:100%;
  font-size: 1rem;
}
* {
  margin: 0;
  padding: 0;
}
ol,ul,li {
    list-style:none;
}
/* 清除浮动 */
.clearfix:before,.clearfix:after {
    content:"";
    display:table;
}
.clearfix:after {
    clear:both;
    overflow:hidden;
}
.clearfix {
    zoom:1; /* for ie6 & ie7 */
}
.clear {
    clear:both;
    display:block;
    font-size:0;
    height:0;
    line-height:0;
    overflow:hidden;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
