<template>
  <div class="login-container">
    <div class="login-box">
      <div class="system-title">行为检测系统</div>
      <div class="system-subtitle">Behavior Detection System</div>

      <el-form :model="form" label-width="0" class="login-form" size="large">
        <el-form-item>
          <el-input
            v-model="form.account"
            placeholder="请输入账号"
            :prefix-icon="User"
          />
        </el-form-item>

        <el-form-item>
          <el-input
            v-model="form.password"
            type="password"
            placeholder="请输入密码"
            show-password
            :prefix-icon="Lock"
          />
        </el-form-item>

        <el-form-item>
          <el-button type="primary" class="login-btn" size="large" @click="submitForm">登 录</el-button>
        </el-form-item>

        <div class="login-footer">重置密码请联系管理员</div>
      </el-form>

      <el-button class="reset-btn" link @click="resetForm">重置</el-button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive } from 'vue'
import { User, Lock } from '@element-plus/icons-vue'
import { login } from '@/api/user'
import { ElMessage } from 'element-plus'
import { useRouter } from 'vue-router'

const router = useRouter();

const form = reactive({
  account: '',
  password: '',
})

const submitForm = () => {
  login(form)
    .then((res: any) => {
      if(res.data.code === 200){
        ElMessage({message: '登录成功', type: 'success', plain: true,})
        localStorage.setItem('userInfo', JSON.stringify(res.data.data))
        router.push('/home')
      }
      else{
        ElMessage({message: '登录失败', type: 'error', plain: true, })
      }
    })
    .catch((err: any) => {
      console.log("登录失败：", err)
      ElMessage({message: '登录失败', type: 'error', plain: true,})
    })
}

const resetForm = () => {
  form.account = ''
  form.password = ''
}
</script>

<style scoped>
/* 整个背景：深色渐变 + 柔光 */
.login-container {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #0f172a 0%, #1e1b4b 50%, #312e81 100%);
  position: relative;
  overflow: hidden;
}

/* 背景装饰：柔光光斑 */
.login-container::before {
  content: '';
  position: absolute;
  width: 600px;
  height: 600px;
  background: radial-gradient(circle, rgba(99, 102, 241, 0.35) 0%, transparent 70%);
  border-radius: 50%;
  top: -250px;
  left: -200px;
  filter: blur(40px);
}

.login-container::after {
  content: '';
  position: absolute;
  width: 500px;
  height: 500px;
  background: radial-gradient(circle, rgba(129, 140, 248, 0.25) 0%, transparent 70%);
  border-radius: 50%;
  bottom: -200px;
  right: -150px;
  filter: blur(40px);
}

/* 登录卡片：玻璃拟态 */
.login-box {
  width: 400px;
  padding: 48px 40px 36px;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.92);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  box-shadow:
    0 20px 60px rgba(0, 0, 0, 0.3),
    inset 0 1px 0 rgba(255, 255, 255, 0.6);
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  z-index: 1;
  animation: fadeUp 0.6s ease;
}

@keyframes fadeUp {
  from {
    opacity: 0;
    transform: translateY(24px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* 标题 */
.system-title {
  font-size: 26px;
  font-weight: 700;
  color: #1e293b;
  letter-spacing: 2px;
  text-align: center;
}

.system-subtitle {
  font-size: 12px;
  color: #94a3b8;
  letter-spacing: 3px;
  text-transform: uppercase;
  margin-top: 8px;
  margin-bottom: 36px;
}

/* 表单 */
.login-form {
  width: 100%;
}

.login-form :deep(.el-input__wrapper) {
  padding: 4px 14px;
  border-radius: 10px;
  background: #f8fafc;
}

/* 登录按钮：通栏 */
.login-btn {
  width: 100%;
  height: 46px;
  font-size: 16px;
  letter-spacing: 6px;
  border-radius: 10px;
}

/* 底部提示 */
.login-footer {
  text-align: center;
  font-size: 12px;
  color: #94a3b8;
}

/* 重置按钮 */
.reset-btn {
  margin-top: 12px;
  color: #94a3b8;
}

.reset-btn:hover {
  color: #3b82f6;
}
</style>
