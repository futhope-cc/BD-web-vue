<template>
  <el-aside :width="isCollapse ? '64px' : '230px'" class="sidebar">
    <div class="logo-section">
      <div class="logo-dot"></div>
      <h2 v-if="!isCollapse">行为检测系统</h2>
      <h2 v-else class="logo-short">BD</h2>
    </div>
    <el-menu
      :default-active="activeMenu"
      class="sidebar-menu"
      :collapse="isCollapse"
      @select="handleMenuSelect"
    >
      <el-menu-item index="1">
        <el-icon><House /></el-icon>
        <span>首页</span>
      </el-menu-item>
      <el-menu-item index="2">
        <el-icon><VideoCamera /></el-icon>
        <span>实时监控</span>
      </el-menu-item>
      <el-menu-item index="3">
        <el-icon><DataAnalysis /></el-icon>
        <span>数据分析</span>
      </el-menu-item>
      <el-menu-item index="4">
        <el-icon><Warning /></el-icon>
        <span>告警记录</span>
      </el-menu-item>
      <el-menu-item index="5">
        <el-icon><User /></el-icon>
        <span>用户管理</span>
      </el-menu-item>
      <el-sub-menu index="6">
        <template #title>
          <el-icon><Folder /></el-icon>
          <span>本地视频</span>
        </template>
        <el-menu-item index="6-1">
          <el-icon><VideoPlay /></el-icon>
          <span>视频管理</span>
        </el-menu-item>
        <el-menu-item index="6-2">
          <el-icon><DataAnalysis /></el-icon>
          <span>识别结果</span>
        </el-menu-item>
      </el-sub-menu>
      <el-menu-item index="8">
        <el-icon><Setting /></el-icon>
        <span>系统设置</span>
      </el-menu-item>
    </el-menu>
  </el-aside>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import { House, VideoCamera, DataAnalysis, Warning, Setting, User, VideoPlay, Folder } from '@element-plus/icons-vue'

const router = useRouter()
const route = useRoute()
const isCollapse = ref(false)

// 根据当前路由计算活跃菜单
const activeMenu = computed(() => {
  const path = route.path
  if (path.includes('/dashboard')) return '1'
  if (path.includes('/monitor')) return '2'
  if (path.includes('/analysis')) return '3'
  if (path.includes('/alert')) return '4'
  if (path.includes('/user')) return '5'
  if (path.includes('/video')) return '6-1'
  if (path.includes('/detectResult')) return '6-2'
  if (path.includes('/settings')) return '8'
  return '1'
})

const handleMenuSelect = (index: string) => {
  const routeMap: Record<string, string> = {
    '1': '/home/dashboard',
    '2': '/home/monitor',
    '3': '/home/analysis',
    '4': '/home/alert',
    '5': '/home/user',
    '6-1': '/home/video',
    '6-2': '/home/detectResult',
    '8': '/home/settings'
  }
  router.push(routeMap[index] as string)
}

// 暴露方法给父组件调用
defineExpose({
  toggleCollapse: () => {
    isCollapse.value = !isCollapse.value
  }
})

</script>

<style scoped>
.sidebar {
  background: linear-gradient(180deg, #0f172a 0%, #1e293b 100%);
  height: 100%;
  overflow-y: auto;
  overflow-x: hidden;
  flex-shrink: 0;
  transition: width 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

/* Logo 区 */
.logo-section {
  height: 64px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
}

.logo-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: linear-gradient(135deg, #3b82f6, #60a5fa);
  box-shadow: 0 0 12px rgba(59, 130, 246, 0.8);
  flex-shrink: 0;
}

.logo-section h2 {
  color: #f1f5f9;
  font-size: 17px;
  margin: 0;
  font-weight: 600;
  letter-spacing: 1px;
  white-space: nowrap;
}

.logo-short {
  font-size: 18px !important;
}

/* 菜单：透明背景 + 圆角高亮项 */
.sidebar-menu {
  border-right: none;
  background: transparent !important;
  padding: 12px 10px;
  --el-menu-bg-color: transparent;
  --el-menu-text-color: #94a3b8;
  --el-menu-active-color: #ffffff;
  --el-menu-hover-bg-color: rgba(255, 255, 255, 0.06);
}

.sidebar-menu :deep(.el-menu-item),
.sidebar-menu :deep(.el-sub-menu__title) {
  height: 46px;
  line-height: 46px;
  border-radius: 10px;
  margin-bottom: 4px;
  transition: all 0.2s ease;
}

.sidebar-menu :deep(.el-menu-item:hover),
.sidebar-menu :deep(.el-sub-menu__title:hover) {
  background: rgba(255, 255, 255, 0.06) !important;
  color: #e2e8f0 !important;
}

/* 激活项：渐变药丸 */
.sidebar-menu :deep(.el-menu-item.is-active) {
  background: linear-gradient(135deg, #3b82f6 0%, #60a5fa 100%) !important;
  color: #fff !important;
  box-shadow: 0 4px 14px rgba(59, 130, 246, 0.4);
}

/* 子菜单激活态 */
.sidebar-menu :deep(.el-sub-menu.is-active > .el-sub-menu__title) {
  color: #e2e8f0 !important;
}

.sidebar-menu :deep(.el-sub-menu .el-menu) {
  background: rgba(0, 0, 0, 0.15) !important;
  border-radius: 10px;
  padding: 4px 0;
}
</style>
