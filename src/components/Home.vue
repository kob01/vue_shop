<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img src="https://i2.hdslb.com/bfs/face/f7c909737e967f8b9600a500d92cde7e4f9f325f.jpg@150w_150h.jpg" />
        <span>后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!-- 页面主题区域 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isCollapse ? '60px' : '200px'">
        <div class="toggle-button" @click="toggleNav">
          |||
        </div>
        <!-- 侧边栏菜单区域 -->
        <el-menu background-color="#545c64" text-color="#fff" active-text-color="#409eff" unique-opened :collapse="isCollapse" :collapse-transition="false" router :default-active="activePath">
          <!-- 一级菜单 -->
          <el-submenu :index="item1.id + ''" v-for="item1 of menulist" :key="item1.id">
            <!-- 一级菜单模板区域 -->
            <template slot="title">
              <!-- 图标 -->
              <i :class="iconsObj[item1.id]"></i>
              <!-- 文本 -->
              <span>{{ item1.authName }}</span>
            </template>
            <!-- 二级菜单 -->
            <el-menu-item :index="'/' + item2.path" v-for="item2 of item1.children" :key="item2.id" @click="saveNavState('/' + item2.path)">
              <!-- 二级菜单模板区域 -->
              <template slot="title">
                <!-- 图标 -->
                <i class="el-icon-menu"></i>
                <!-- 文本 -->
                <span>{{ item2.authName }}</span>
              </template></el-menu-item
            >
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧内容主题 -->
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
      // 左侧菜单数据
      menulist: [],
      iconsObj: {
        '125': 'iconfont icon-user',
        '103': 'iconfont icon-tijikongjian',
        '101': 'iconfont icon-shangpin',
        '102': 'iconfont icon-danju',
        '145': 'iconfont icon-baobiao'
      },
      // 是否折叠
      isCollapse: false,
      // 被激活的链接地址
      activePath: ''
    }
  },
  created() {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // 获取所有菜单
    async getMenuList() {
      const { data: res } = await this.$http.get('/menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
    },
    // 切换菜单折叠与展开
    toggleNav() {
      this.isCollapse = !this.isCollapse
    },
    // 保持链接激活状态
    saveNavState(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}
</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
}
.el-header {
  background-color: #373d41;
  color: #fff;
  font-size: 20px;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  > div {
    display: flex;
    align-items: center;
    img {
      width: 60px;
      height: 60px;
    }
    span {
      margin-left: 20px;
    }
  }
}

.el-aside {
  background-color: #545c64;
  color: #fff;
  transition: width 0.2s;
  .toggle-button {
    font-size: 10px;
    height: 30px;
    line-height: 30px;
    color: #fff;
    text-align: center;
    letter-spacing: 0.2em;
    cursor: pointer;
  }
  .toggle-button:hover {
    background-color: #434a50;
  }
  .el-menu {
    border-right: 0;
  }
}

.el-main {
  background-color: #e9eef3;
  color: #333;
  text-align: center;
}

.iconfont {
  margin-right: 10px;
}
</style>
