<script setup lang="ts">
import QmTree from "./qmtree.vue";
</script>

<template>
  <el-row :gutter="10">
    <el-col :xs="4" :sm="6" :md="8" :lg="23" :xl="20">      
      <div class="grid-content ep-bg-purple" />
      <QmTree />
    </el-col>
  </el-row>
</template>

<style>
.el-col {
  border-radius: 4px;
}

.grid-content {
  border-radius: 4px;
  min-height: 100%;
  min-width: 100%;
}
</style>
