<script>
import TopNavbar from '@/components/TopNavbar.vue';
import JSConfetti from 'js-confetti'
const confetti = new JSConfetti()
function showConfetti() {
  confetti.addConfetti()
}
confetti.addConfetti({
   emojis: ['🌈', '⚡️', '💥', '✨', '💫', '🌸','💃','🕶'],
   emojiSize: 20,
})
showConfetti()
export default {
  components: {
    TopNavbar
  },
  // 其他选项...
};
</script>


<template>
  <div id="app">
    <TopNavbar />
    <router-view />
  </div>
  
</template>



<style>

h1 {
  text-align: center;
  cursor: pointer;
  margin-top: 3em;
}
#app {
  /* 给予顶部一个足够的空间，等于TopNavbar的高度 */
  /* 这个值应该与TopNavbar组件的实际高度相匹配 */
  padding-top: 70px; /* 这是一个示例值，根据TopNavbar的实际高度进行调整 */
}

/* 其他全局样式 */
</style>
