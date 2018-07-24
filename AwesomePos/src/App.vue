<template>
  <div id="app">
    <!-- 左侧栏 -->
    <leftNav></leftNav>
    <transition name="fade" mode="out-in">
      <router-view/>
    </transition>
  </div>
</template>

<script>
import leftNav from '@/components/common/leftNav'
export default {
  name: 'App',
  data () {
    return {

    }
  },
  components: {
    leftNav
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  
}
.fade-enter{
  opacity: 0;
}
.fade-enter-active {
  transition: opacity .5s;
}
.fade-leave {
  opacity: 1;
}
.fade-leave-active {
  opacity: 0;
  transition: .5s;
}
.el-tabs--border-card>.el-tabs__header .el-tabs__item.is-active {
  color: #ff7100;
}
</style>
