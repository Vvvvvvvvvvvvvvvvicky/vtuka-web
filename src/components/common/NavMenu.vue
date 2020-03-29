<template>
  <div class="manage_page fillcontain"> 
      <el-row style="height: 100%;">
          <el-col :span="4"  style="min-height: 100%; ">
              <el-radio-group v-model="isCollapse" style="margin-bottom: 20px;">
                  <el-radio-button :label="false">展开</el-radio-button>
                  <el-radio-button :label="true">收起</el-radio-button>
              </el-radio-group>
              <el-menu
              default-active="1-1"
              router
              class="el-menu-vertical-demo"
              :collapse="isCollapse">
                <el-submenu index="1">
                    <template slot="title">
                        <i class="el-icon-menu"></i>
                        <span>搭配</span>
                    </template>
                    <el-menu-item-group>
                        <el-menu-item index="1-1">搭配清单</el-menu-item>
                        <el-menu-item index="1-2">运营清单</el-menu-item>
                    </el-menu-item-group>
                </el-submenu>
                <el-submenu index="2">
                    <template slot="title">
                        <i class="el-icon-menu"></i>
                        <span>用户</span>
                    </template>
                    <el-menu-item-group>
                        <el-menu-item index="2-1">用户清单</el-menu-item>
                        <el-menu-item index="2-2">用户图表</el-menu-item>
                    </el-menu-item-group>
                </el-submenu>
              </el-menu>
      </el-col>
      <el-col :span="20" style="height: 100%;overflow: auto;">
          <keep-alive>
              <router-view></router-view>
          </keep-alive>
			</el-col>
      </el-row>
  </div>
</template>

<style>
  .el-menu-vertical-demo:not(.el-menu--collapse) {
    width: 200px;
    min-height: 400px;
  }
</style>

<script>
   export default {
    data() {
      return {
        isCollapse: false,
      };
    },
    computed: {
			defaultActive: function(){
				return this.$route.path.replace('/', '');
			}
		},
  }
</script>