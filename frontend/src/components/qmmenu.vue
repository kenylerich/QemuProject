<template>
  <el-tabs v-model="activeName" class="main-menu-tabs" @tab-click="handleClick">
    <el-tab-pane label="VirtualPC Management" name="first"
      >Disk Management</el-tab-pane
    >
    <el-tab-pane label="Virtual Machine Settings" name="second"
      >Virtual Machine Settings</el-tab-pane
    >
  </el-tabs>
</template>
<script lang="ts" setup>
import { ref } from "vue";
import type { TabsPaneContext } from "element-plus";

const activeName = ref("first");

const handleClick = (tab: TabsPaneContext, event: Event) => {
  console.log(tab, event);
};
</script>

<style>
.main-menu-tabs > .el-tabs__content {
  padding: 16px;
  color: var(--el-text-color-primary-light);
  font-size: 24px;
  font-weight: 600;
}
.el-tabs__item {
  color: #909399;
}
</style>
