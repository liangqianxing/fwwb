<template>
  <div id="app">
    <TopNavbar />
    <router-view />
  </div>
</template>

<script>
import TopNavbar from '@/components/TopNavbar.vue';

export default {
  components: {
    TopNavbar
  },
  // 其他选项...
};
</script>

<style>
#app {
  /* 给予顶部一个足够的空间，等于TopNavbar的高度 */
  /* 这个值应该与TopNavbar组件的实际高度相匹配 */
  padding-top: 70px; /* 这是一个示例值，根据TopNavbar的实际高度进行调整 */
}

/* 其他全局样式 */
</style>
