<template>
  <el-container class="layout-container">
    <el-header class="el-header">
      <el-dropdown>
        <span class="el-dropdown__box">
          <div>
            <strong>当前用户：</strong>{{ loginUser.nickName }}
          </div>
          <el-icon>
            <ArrowDownBold />
          </el-icon>
          <!-- <el-icon><Lock /></el-icon> -->
        </span>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item @click="logout" :icon="SwitchButton">退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </el-header>
    <el-main class="layout-bottom-box">
      <div class="left">
        <el-aside width="200px" class="el-aside">
          <el-menu class="el-menu" router>
            <el-menu-item index="/oj/layout/cuser">
              <el-icon>
                <Management />
              </el-icon>
              <span>用户管理</span>
            </el-menu-item>
            <el-menu-item index="/oj/layout/question">
              <el-icon>
                <Management />
              </el-icon>
              <span>题目管理</span>
            </el-menu-item>
            <el-menu-item index="/oj/layout/exam">
              <el-icon>
                <Management />
              </el-icon>
              <span>竞赛管理</span>
            </el-menu-item>
          </el-menu>
        </el-aside>
      </div>
      <div class="right">
        <RouterView />
      </div>
    </el-main>

  </el-container>
</template>

<script setup>
import {
  Management,
  ArrowDownBold,
  Lock,
  SwitchButton
} from '@element-plus/icons-vue'
import { reactive } from 'vue'
import router from '@/router'
import { getUserInfoService,logoutService } from '@/apis/suser'
import { removeToken } from '@/utils/cookie'

const loginUser = reactive({
  nickName: ''
})

async function getUserInfo() {
  const userInfo = await getUserInfoService()
  loginUser.nickName = userInfo.data.nickName
}
getUserInfo()

async function logout() {
  await ElMessageBox.confirm(
    '确认退出',  
    '温馨提示',
    {
      confirmButtonText: '确认',
      cancelButtonText: '退出',
      type: 'warning',
    }
  )
  // try {
  // }catch {
  // }
  await logoutService()
  removeToken()
  router.push("/oj/login")
}



</script>

<style lang="scss" scoped>
.layout-container {
  height: 100vh;
  background: #f7f7f7;

  .layout-bottom-box {
    display: flex;
    justify-content: space-between;
    height: calc(100vh - 100px);
    overflow: hidden;

    .left {
      margin-right: 20px;
      background: #fff;
      display: flex;

      :deep(.el-menu) {
        flex: 1;

        .el-menu-item.is-active {
          color: #32c5ff;
        }

        .el-menu-item:hover {
          background: #fff;
          color: #32c5ff;
        }
      }
    }

    .right {
      flex: 1;
      overflow-y: auto;
      background: #fff;
      padding: 20px;
    }
  }

  .el-aside {
    background-color: #fff;

    &__logo {
      height: 120px;

    }

    .el-menu {
      border-right: none;
    }
  }

  .el-header {
    background-color: #fff;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    height: 40px;

    .el-dropdown__box {
      display: flex;
      align-items: center;

      .el-icon {
        color: #4c4141;
        margin-left: 20px;
      }

      &:active,
      &:focus {
        outline: none;
      }
    }
  }

  .el-footer {
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 14px;
    color: #666;
  }
}
</style>
