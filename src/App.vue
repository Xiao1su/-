<template>
  <BDTopBar/>
  <TopBarStay/>
  <BDMid/>
  <BDBottom/>
  <BDEnd/>
</template>

<script>
import BDTopBar from './components/BDTopBar.vue'
import TopBarStay from './components/TopBarStay.vue'
import BDMid from './components/BDMid.vue'
import BDBottom from './components/BDBottom.vue'
import BDEnd from './components/BDEnd.vue'

export default {
  name: 'App',
  components: {
    BDTopBar,
    TopBarStay,
    BDMid,
    BDBottom,
    BDEnd
  },
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
  min-width: 1250px;
  cursor: default;
}
</style>
