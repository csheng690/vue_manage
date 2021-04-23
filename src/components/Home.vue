<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img class="imgs" src="../assets/OIP.jpg" alt="" />
        <span>项目管理</span>
      </div>
      <el-button type="info" @click="logout" class="btnout">
        退出登录
      </el-button>
    </el-header>
    <!-- 页面主体 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <div class="toggle-btn" @click="toggleCollapse">|||</div>
        <!-- 侧边栏菜单 -->
        <el-menu
          background-color="#6ef6f1"
          text-color="#333"
          active-text-color="#ffd04b"
          unique-opened
          :collapse="isCollapse"
          :collapse-transition="false"
          router
          :default-active="activePath"
        >
          <!-- 一级菜单 -->
          <el-submenu
            :index="item.id + ''"
            v-for="item in menulist"
            :key="item.id"
          >
            <template #title>
              <!-- 图标 -->
              <i class="el-icon-location"></i>
              <!-- 文本 -->
              <span>{{ item.authName }}</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item
              :index="'/' + subItem.path"
              v-for="subItem in item.children"
              :key="subItem.id"
              @click="saveNavState('/' + subItem.path)"
            >
              <template #title>
                <!-- 图标 -->
                <i class="el-icon-location"></i>
                <!-- 文本 -->
                <span>{{ subItem.authName }}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 内容 -->
      <el-main>
        <!-- 路由占位符 -->
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      //左侧菜单数据
      menulist: [],
      // 是否折叠
      isCollapse: false,
      // 被激活的链接地址
      activePath: "",
    };
  },
  created() {
    this.getMenuList();
    this.activePath = window.sessionStorage.getItem("activePath");
  },
  methods: {
    logout() {
      window.sessionStorage.clear();
      this.$router.push("/login");
    },
    // 获取所有的菜单
    async getMenuList() {
      const { data: res } = await this.$http.get("menus");
      if (res.meta.status !== 200)
        return this.$message.console.error(res, meta, msg);
      this.menulist = res.data;
    },
    // 切换菜单折叠展开
    toggleCollapse() {
      this.isCollapse = !this.isCollapse;
    },
    // 保存链接的激活状态
    saveNavState(activePath) {
      window.sessionStorage.setItem("activePath", activePath);
      this.activePath = activePath;
    },
  },
};
</script>

<style Lang="less" scoped>
.home-container {
  height: 100%;
}
.el-header {
  background-color: #4fb2a1;
  display: flex;
  justify-content: space-between;
  box-shadow: 0 0 10px #333;
  align-items: center;
  color: #fff;
  font-size: 20px;
  z-index: 10;
}
.el-header div {
  display: flex;
  align-items: center;
}
.el-header div span {
  margin-left: 15px;
}
.imgs {
  height: 50px;
  padding: 6px;
  box-shadow: 0 0 10px #fff inset;
  box-sizing: border-box;
  border-radius: 50%;
}
.btnout {
  height: 60%;
  background-color: #4fb2a1;
  border: none;
}
.el-aside {
  transition: width 1s;
  background-color: #6ef6f1;
  box-shadow: 0 0 10px #333;
}
.toggle-btn {
  background-color: #6ef6f1;
  font-size: 10px;
  line-height: 24px;
  color: #333;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
.el-menu {
  border: none;
}
</style>
