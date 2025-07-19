<template>
  <div class="login-page">
    <!-- 背景装饰元素 -->
    <div class="orange"></div>
    <div class="blue"></div>
    <div class="blue small"></div>
    
    <!-- 登录主容器 -->
    <div class="login-box">
      <!-- 系统名称区域 -->
      <div class="logo-box">
        <div class="right">
          <div class="sys-name">OJ后台管理</div>
          <div class="sys-sub-name">帮助100万学生就业</div>
        </div>
      </div>
      
      <!-- 登录表单区域 -->
      <div class="form-box">
        <!-- 账号输入项 -->
        <div class="form-item">
          <img src="../assets/images/shouji.png">
          <el-input v-model="userAccount" placeholder="请输入账号" />
        </div>
        
        <!-- 密码输入项 -->
        <div class="form-item">
          <img src="../assets/images/yanzhengma.png">
          <el-input 
            v-model="password"  
            type="password" 
            show-password 
            placeholder="请输入密码" 
          />
        </div>
        
        <!-- 登录按钮 -->
        <div class="submit-box" @click="loginFun">
          登录
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// 导入依赖
import { ref } from 'vue'
import { loginService } from '@/apis/suser'
import { setToken } from '@/utils/cookie'
import router from '@/router'

// 响应式变量
const userAccount = ref('')  // 账号
const password = ref('')     // 密码

// 登录函数
async function loginFun() {
  try {
    // 调用登录接口
    const loginResult = await loginService(userAccount.value, password.value)
    console.log("loginResult:", loginResult)
    
    // 存储Token并跳转页面
    router.push("/oj/layout/question")
    setToken(loginResult.data)
  } catch (error) {
    // 错误处理
    console.log("error:", error)
  }
}
</script>

<style lang="scss" scoped>
.login-page {
  width: 100vw;
  height: 100vh;
  position: relative;
  overflow: hidden;

  // 登录框样式
  .login-box {
    width: 456px;
    height: 404px;
    background: #FFFFFF;
    box-shadow: 0px 0px 6px 0px rgba(0, 0, 0, 0.1);
    border-radius: 10px;
    opacity: 0.9;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 2;
    padding: 0 72px;
    padding-top: 50px;
    overflow: hidden;

    // 系统名称样式
    .logo-box {
      display: flex;
      align-items: center;
      margin-bottom: 30px;

      img {
        width: 68px;
        height: 68px;
        margin-right: 16px;
      }

      .sys-name {
        height: 33px;
        font-family: PingFangSC, PingFang SC;
        font-weight: 600;
        font-size: 24px;
        color: #222222;
        line-height: 33px;
        margin-bottom: 13px;
      }

      .sys-sub-name {
        height: 22px;
        font-family: PingFangSC, PingFang SC;
        font-weight: 400;
        font-size: 16px;
        color: #222222;
        line-height: 22px;
      }
    }

    // 表单区域样式
    :deep(.form-box) {
      // 登录按钮样式
      .submit-box {
        margin-top: 90px;
        width: 456px;
        height: 48px;
        background: #32C5FF;
        border-radius: 8px;
        cursor: pointer;
        display: flex;
        justify-content: center;
        align-items: center;
        font-family: PingFangSC, PingFang SC;
        font-weight: 600;
        font-size: 16px;
        color: #FFFFFF;
        letter-spacing: 1px;
      }

      // 输入项样式
      .form-item {
        display: flex;
        align-items: center;
        width: 456px;
        height: 48px;
        background: #F8F8F8;
        border-radius: 8px;
        margin-bottom: 30px;
        position: relative;

        // 验证码按钮样式（预留）
        .code-btn-box {
          position: absolute;
          right: 0;
          width: 151px;
          height: 48px;
          background: #32C5FF;
          border-radius: 8px;
          top: 0;
          display: flex;
          align-items: center;
          justify-content: center;
          cursor: pointer;

          span {
            font-family: PingFangSC, PingFang SC;
            font-weight: 400;
            font-size: 16px;
            color: #FFFFFF;
          }
        }

        // 错误提示样式（预留）
        .error-tip {
          position: absolute;
          width: 140px;
          text-align: right;
          padding-right: 12px;
          height: 20px;
          font-family: PingFangSC, PingFang SC;
          font-weight: 400;
          font-size: 14px;
          color: #FD4C40;
          line-height: 20px;
          right: 0;

          &.bottom {
            right: 157px;
          }
        }

        // 输入框样式
        .el-input {
          width: 380px;
          font-family: PingFangSC, PingFang SC;
          font-weight: 400;
          font-size: 16px;
          color: #222222;
        }

        .el-input__wrapper {
          border: none;
          box-shadow: none;
          background: transparent;
          width: 230px;
          padding-left: 0;
        }

        // 图标样式
        img {
          width: 24px;
          height: 24px;
          margin: 0 18px;
        }
      }
    }
  }

  // 半透明遮罩层
  &::after {
    position: absolute;
    top: 0;
    left: 0;
    height: 100vh;
    bottom: 0;
    right: 0;
    background: rgba(255, 255, 255, .8);
    z-index: 1;
    content: '';
  }

  // 背景装饰 - 橙色圆形
  .orange {
    background: #F0714A;
    width: 498px;
    height: 498px;
    border-radius: 50%;
    opacity: 0.67;
    filter: blur(50px);
    left: 14.2%;
    top: 41%;
    position: absolute;
  }

  // 背景装饰 - 蓝色圆形
  .blue {
    width: 334px;
    height: 334px;
    background: #32C5FF;
    opacity: 0.67;
    filter: blur(50px);
    position: absolute;
    top: 16.3%;
    left: 80.7%;

    // 小蓝色圆形
    &.small {
      width: 186px;
      height: 186px;
      top: 8.2%;
      left: 58.2%;
    }
  }
}
</style>