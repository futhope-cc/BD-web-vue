<template>
  <el-header class="header">
    <div class="header-left">
      <el-icon class="menu-icon" @click="handleToggleSidebar"><Expand /></el-icon>
      <el-breadcrumb separator="/">
        <el-breadcrumb-item>{{ currentPageName }}</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="header-right">
      <div class="icon-btn-wrap">
        <el-icon class="header-icon" @click="handleBellClick"><Bell /></el-icon>
        <span class="badge-dot"></span>
      </div>
      <el-dropdown>
        <div class="user-info">
          <el-avatar :size="35" src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png" />
          <span class="username">{{ userInfo.username }}</span>
          <el-icon><ArrowDown /></el-icon>
        </div>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item>个人中心</el-dropdown-item>
            <el-dropdown-item>修改密码</el-dropdown-item>
            <el-dropdown-item divided @click="handleLogout">退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </div>
  </el-header>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import { Expand, Bell, ArrowDown } from '@element-plus/icons-vue'
import { ElMessage } from 'element-plus'
const router = useRouter()
const route = useRoute()
const userInfo = ref(JSON.parse(localStorage.getItem('userInfo') || "{'username':'未知'}"))

// 路由名称映射
const routeNameMap: Record<string, string> = {
  '/': '首页',
  '/home': '首页',
  '/home/dashboard': '首页',
  '/login': '登录',
  '/home/monitor': '实时监控',
  '/home/analysis': '数据分析',
  '/home/alert': '告警记录',
  '/home/settings': '系统设置',
  '/home/user': '用户管理',
  '/home/video': '视频管理',
  '/home/detectResult': '识别结果',
}
// 实时计算当前页面名称
const currentPageName = computed(() => {
  // 其次根据路由推算
  return routeNameMap[route.path] || '首页'
})

// 定义 emit
const emit = defineEmits(['toggle-sidebar', 'click-dialog'])

const handleToggleSidebar = () => {
  emit('toggle-sidebar')
}

const handleLogout = () => {
  router.push('/login');
  localStorage.removeItem('userInfo');
  ElMessage({message: '已退出登录', type: 'primary', plain: true,})
  console.log('已退出登录');
}

const handleBellClick = () => {
  emit('click-dialog')
}
</script>

<style scoped>
.header {
  height: 64px;
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 24px;
  border-bottom: 1px solid #f1f5f9;
  z-index: 10;
  flex-shrink: 0;
}

.header-left {
  display: flex;
  align-items: center;
  gap: 20px;
}

.menu-icon {
  font-size: 20px;
  cursor: pointer;
  color: #475569;
  padding: 8px;
  border-radius: 8px;
  transition: all 0.2s ease;
}

.menu-icon:hover {
  color: #3b82f6;
  background: #eff6ff;
}

.header-right {
  display: flex;
  align-items: center;
  gap: 16px;
}

/* 图标按钮容器（含红点） */
.icon-btn-wrap {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

.header-icon {
  font-size: 18px;
  cursor: pointer;
  color: #475569;
  padding: 9px;
  border-radius: 10px;
  transition: all 0.2s ease;
}

.header-icon:hover {
  color: #3b82f6;
  background: #eff6ff;
}

.badge-dot {
  position: absolute;
  top: 6px;
  right: 6px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #f43f5e;
  border: 2px solid #fff;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  padding: 5px 10px;
  border-radius: 10px;
  transition: background 0.2s ease;
}

.user-info:hover {
  background: #f8fafc;
}

.username {
  font-size: 14px;
  color: #334155;
  font-weight: 500;
}
</style>
