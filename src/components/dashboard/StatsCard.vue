<template>
  <el-card class="stat-card" :class="`card-${color}`">
    <div class="stat-content">
      <div class="stat-icon">
        <component :is="icon" :size="40" />
      </div>
      <div class="stat-info">
        <div class="stat-value">{{ value }}</div>
        <div class="stat-label">{{ label }}</div>
      </div>
    </div>
  </el-card>
</template>

<script setup lang="ts">
import type { Component } from 'vue'

interface Props {
  icon: Component
  value: string | number
  label: string
  color: 'blue' | 'green' | 'orange' | 'purple'
}

defineProps<Props>()
</script>

<style scoped>
.stat-card {
  border-radius: 16px;
  border: none;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: default;
}

.stat-card:hover {
  transform: translateY(-4px);
}

.stat-content {
  display: flex;
  align-items: center;
  gap: 18px;
}

/* 图标：浅色底 + 彩色图标 */
.stat-icon {
  width: 56px;
  height: 56px;
  border-radius: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.card-blue .stat-icon {
  background: #eff6ff;
  color: #3b82f6;
}

.card-green .stat-icon {
  background: #ecfdf5;
  color: #10b981;
}

.card-orange .stat-icon {
  background: #fff7ed;
  color: #f59e0b;
}

.card-purple .stat-icon {
  background: #ecfeff;
  color: #06b6d4;
}

.stat-info {
  flex: 1;
  min-width: 0;
}

.stat-value {
  font-size: 26px;
  font-weight: 700;
  color: #1e293b;
  margin-bottom: 4px;
  letter-spacing: -0.5px;
}

.stat-label {
  font-size: 13px;
  color: #94a3b8;
}
</style>
