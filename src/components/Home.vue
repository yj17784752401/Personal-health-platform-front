<template>
  <!-- 引入container布局 -->
  <el-container class="home-container">
    <!-- 头部 -->
    <el-header>
      <div>
        <img src="../assets/logo.jpg" alt="#" />
        <span>个人运动平台</span>
      </div>

      <el-button type="info" @click="logout">安全退出</el-button></el-header
    >
    <!-- 主体 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="iscollapse ? '64px' : '200px'">
        <div class="toggle-button" @click="toggleCollapase">|||</div>
        <el-menu
          background-color="#545c64"
          text-color="#fff"
          active-text-color="#409eff"
          unique-opened
          :collapse="iscollapse"
          :collapse-transition="false"
          :router="true"
          :default-active="activePath"
        >
          <!-- 一级菜单 -->
          <el-submenu
            :index="item.id + ''"
            v-for="item in menuList"
            :key="item.id"
          >
            <template slot="title">
              <i :class="iconsObject[item.id]"></i>
              <span>{{ item.title }}</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item
              :index="it.path"
              v-for="it in item.sList"
              :key="it.id"
              @click="saveNavState(it.path)"
            >
              <template slot="title">
                <i :class="iconsObject[it.id]"></i>
                <span>{{ it.title }}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 主体内容 -->
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>
<script>
export default {
  data() {
    return {
      // 菜单列表
      menuList: [],
      iscollapse: false, //伸缩属性
      iconsObject: {
        100: "iconfont icon-denglu",
        200: "iconfont icon-yundong",
        101: "iconfont icon-liebiao",
        102: "iconfont icon-guanliyuan",
        103: "iconfont icon-yundong",
        104: "iconfont icon-shangpin",
        201: "iconfont icon-shu",
        202: "iconfont icon-qialuli",
        203: "iconfont icon-shiwu",
        204: "iconfont icon-shiwu",
      },
      activePath: "/welcome", //默认路径
    };
  },
  // 初始化时事件
  created() {
    // 查询menuList
    this.getMenuList();
    this.activePath = window.sessionStorage.getItem("activePath");
  },
  methods: {
    logout() {
      //安全退出
      window.sessionStorage.clear(); //清楚session
      this.$router.push("/login"); //回到登录页
    },
    // 获取导航菜单方法
    async getMenuList() {
      const { data: res } = await this.$http.get("menus");
      if (res.flag != 200) return this.$message.error("获取列表失败！！！！"); //访问失败
      this.menuList = res.menus; //访问成功后数据回填
    },
    // 控制伸缩
    toggleCollapase() {
      this.iscollapse = !this.iscollapse;
    },
    //保存路径
    saveNavState(activePath) {
      window.sessionStorage.setItem("activePath", activePath);

      this.activePath = activePath;
    },
  },
};
</script>
<style lang="less" scoped>
.home-container {
  height: 100%;
}
// 头部样式
.el-header {
  background-color: #343d41;
  display: flex;
  justify-content: space-between;
  padding-left: 0%;
  align-items: center;
  color: #fff;
  font-size: 20px;
  div {
    display: flex;
    align-items: center;
    span {
      margin-left: 15px;
    }
  }
}
// 侧边栏样式
.el-aside {
  background-color: #333744;
  .el-menu {
    border-right: none;
  }
}
// 主体样式
.el-main {
  background-color: #eaedf1;
}
img {
  width: 55px;
  height: 55px;
}
// |||按钮样式
.toggle-button {
  background-color: #4a5064;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer; //显示小手
}
</style>
