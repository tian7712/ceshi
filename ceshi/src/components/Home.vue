<template>
  <el-container class="home_container">
    <!-- 页面主体区域 -->

    <!-- 侧边栏 -->
    <el-aside :width="isCollaps ? '64px' : '200px'">
      <div class="home-aside">
        <span>系统测试</span>
      </div>
      <!-- router 开启路由，二级菜单通过index值作为跳转 -->
      <el-menu
        background-color="#333744"
        text-color="#fff"
        active-text-color="#409EFF"
        unique-opened
        :collapse="isCollaps"
        :collapse-transition="false"
        router
        :default-active="activePath"
      >
        <!-- 在index属性上加：，动态绑定，设置成唯一值，这样一级菜单点开的时候只会显示自己的二级菜单，index属性需要字符串类型的值，所以可以拼接空字符串的方式转换 -->
        <el-submenu :index="item.id + ''" :key="item.id" v-for="item in menulist">
          <template slot="title">
            <i :class="iconObj[item.id]"></i>
            <span>{{item.authName}}</span>
          </template>
          <el-menu-item
            :index="'/' + subItem.path + ''"
            v-for="subItem in item.children"
            :key="subItem.id"
            @click="saveNavStatus('/' + subItem.path)"
          >
            <!-- 点击了二级菜单，需要将当前的路径保存起来，存储到sessionStorage ，然后在赋值给:default-active 属性，这样的话，只要不关闭浏览器或者不点击别的二级菜单之前的状态都会保存下来 -->
            <template slot="title">
              <i class="el-icon-menu"></i>
              <span>{{subItem.authName}}</span>
            </template>
          </el-menu-item>
        </el-submenu>
      </el-menu>
    </el-aside>
    <!-- 右侧主体区域 -->
    <el-main>
      <el-row>
        <el-col :span="18" class="right-row"> 1111</el-col>
        <el-col :span="1">
          <el-badge :value="7" class="item" :width="'10px'">
            <i class="el-icon-bell"></i>
          </el-badge>
        </el-col>
        <el-col :span="1">
          <el-avatar src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"></el-avatar>
        </el-col>
        
        <el-col :span="2">
          <el-dropdown trigger="click">
            
            <span class="el-dropdown-link">
              {{shuju.username}}
              <i class="el-icon-caret-bottom el-icon--right"></i>
            </span>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item class="clearfix">
                <div @click="logout">退出</div>
              </el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </el-col>
        <el-col :span="2">
          <el-button type="info" @click="logout">退出</el-button>
        </el-col>
      </el-row>
       <el-button type="primary" @click="indexz" class="index-button">主页</el-button>

      <router-view></router-view>
      <!-- // 路由的占位 -->
    </el-main>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      menulist: [],
      iconObj: {
        '125': 'iconfont icon-user',
        '103': 'iconfont icon-tijikongjian',
        '101': 'iconfont icon-shangpin',
        '102': 'iconfont icon-danju',
        '145': 'iconfont icon-baobiao'
      },
      isCollaps: false,
      // 被激活的链接地址
      activePath: '',
      shuju: {
        username: window.sessionStorage.username
        // console.log(ab)
      }
    }
  },
  // 实例创建完成之后
  created() {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout() {
      // 清空token

      window.sessionStorage.clear()
      // 重定向页面到登录
      this.$router.push('/login')
    },
    indexz() {

      // 跳转到主页
      this.$router.push('/welcome')

    },
    // 获取菜单列表
    async getMenuList() {
      const { data: res } = await this.$http('/menus')
      // console.log(res)
      if (res.meta.status !== 200) return this.$message.error(res.meta.message)
      this.menulist = res.data
    },
    
    // 保存链接的激活状态
    saveNavStatus(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}
</script>

<style lang="less" scoped>
.home_container {
  height: 100%;
  .home-aside{
    text-align: center;
    height: 50px;
    line-height: 50px;
    color: white;
    font-weight: bold;
  }
}
.el-header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
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
.el-aside {
  background-color: #333744;
  .el-menu {
    border-right: 0;
  }
}
.el-main {
  background-color: #eaedf1;
}
.iconfont {
  margin-right: 10px;
}
.right-row{
  visibility:hidden;
}
// Layout 布局
.el-row {
  margin-bottom: 20px;
  &:last-child {
    margin-bottom: 0;
  }
}
.el-col {
  border-radius: 4px;
}
.bg-purple-dark {
  background: #99a9bf;
}
.bg-purple {
  background: #d3dce6;
}
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}
.el-dropdown-link {
  color: black;
  height: 40px;
  line-height: 40px;
}
.item {
  margin-top: 5px;
  margin-right: 20px;
  // width: 12px;
  // height: 12px;
}
// icon
.el-icon-bell {
  color: black;
  // position: relative;
}
.index-button{
  margin: 10px;
}

</style>
