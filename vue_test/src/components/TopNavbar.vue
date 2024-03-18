<template>
  <nav class="top-navbar">
    <!-- Logo和应用名称 -->
    <div class="top-logo-name">
      <img src="@/assets/top-logo.png" alt="Logo" class="top-logo" />
      <h1 class="app-name">歌声转换</h1>
    </div>

    <!-- 导航菜单 -->
    <ul class="top-nav-menu">
      <li>
        <img src="@/assets/top-product.png" alt="产品服务" class="top-menu-icon" />
        <router-link to="/">产品服务</router-link>
      </li>
      <li>
        <img src="@/assets/top-my-music.png" alt="我的音乐" class="top-menu-icon" />
        <router-link to="/feature1">我的音乐</router-link>
      </li>
      <li>
        <img src="@/assets/top-we.png" alt="关于我们" class="top-menu-icon" />
        <router-link to="/feature2">关于我们</router-link>
      </li>
    </ul>

    <!-- 搜索框和按钮 -->
    <div class="search-box">
      <input type="search" placeholder="搜索内容" v-model="searchQuery" @keyup.enter="search" />
      <button @click="search">
        <img src="@/assets/top-search-button.png" alt="Search" class="search-button" />
      </button>
    </div>

    <!-- 用户图标 -->
    <div class="top-user-icon">
      <img src="@/assets/top-user.png" alt="User Settings" />
    </div>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: ' ',
    };
  },
  methods: {
    search() {
      console.log('Searching for:', this.searchQuery);
    },
  },
};
</script>

<style scoped>
.top-navbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #6A8D6D;
  color: white;
  padding: 10px 20px;
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  box-sizing: border-box;
  z-index: 1000;
}

.top-logo-name {
  display: flex;
  align-items: center;
  flex-grow: 0.5;
}

.top-logo {
  height: 80px;
}

.app-name {
  margin-left: 10px;
  font-size: 1.5em;
}

.top-nav-menu {
  list-style-type: none;
  display: flex;
  padding: 0;
  font-size: 16px;
  margin-left: auto;
  margin-right: 2%;
}

.top-nav-menu li {
  margin-right: 50px;
  color: white;
}

.top-menu-icon {
  width: 20px;
  height: 20px;
  margin-right: 10px;
}

.search-box {
  display: flex;
  justify-content: flex-end;
  flex-grow: 2;
}

.search-box input {
  padding: 5px 10px;
  color: black;
  background-color: white;
  border: 1px solid #ffffff;
  margin-right: 6px;
}

.search-box button {
  background: none;
  border: none;
  cursor: pointer;
}

.search-button {
  width: 35px;
  height: 35px;
}

.top-user-icon {
  cursor: pointer;
  padding-left: 50px;
  
}

/* 以下是路由链接的样式 */
.router-link-active {
  color: white;
  text-decoration: none;
}

/* 其他可能需要的样式 */
</style>



