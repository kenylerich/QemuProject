<template>
  <div class="custom-tree-container">
    <p>Using render-content</p>
    <el-tree
      style="max-width: 600px"
      :data="dataSource"
      show-checkbox
      node-key="id"
      default-expand-all
      :expand-on-click-node="false"
      :render-content="renderContent"
    />
    <p>Using scoped slot</p>
    <el-tree
      style="max-width: 600px"
      :data="dataSource"
      show-checkbox
      node-key="id"
      default-expand-all
      :expand-on-click-node="false"
    >
      <template #default="{ node, data }">
        <span class="custom-tree-node">
          <span>{{ node.label }}</span>
          <span>
            <a @click="append(data)"> Append </a>
            <a style="margin-left: 8px" @click="remove(node, data)"> Delete </a>
          </span>
        </span>
      </template>
    </el-tree>
  </div>
</template>

<script lang="ts" setup>
import { ref } from "vue";
import type Node from "element-plus/es/components/tree/src/model/node";

interface Tree {
  id: number;
  label: string;
  children?: Tree[];
}

let idSet = new Set<number>([1000]);
let currentId = 1000;

const getNextId = (): number => {
  let nextId = currentId;
  while (idSet.has(nextId)) {
    nextId++;
  }
  idSet.add(nextId);
  currentId = nextId;
  return nextId;
};

const append = (data: Tree) => {
  if (!data) {
    console.error('Data is not defined');
    return;
  }

  try {
    const newChild = { id: getNextId(), label: "testtest-" + newChild.id, children: [] };
    if (!data.children) {
      data.children = [];
    }
    data.children.push(newChild);
    dataSource.value = [...dataSource.value];
  } catch (error) {
    console.error('Error appending node:', error);
  }
};

const modify = (node: Node, data: Tree) => {
  if (!node || !data) {
    console.error('Node or data is not defined');
    return;
  }
}

const remove = (node: Node, data: Tree) => {
  if (!node || !data) {
    console.error('Node or data is not defined');
    return;
  }

  try {
    const parent = node.parent;
    const children: Tree[] = parent.data.children || parent.data;
    const index = children.findIndex((d) => d.id === data.id);
    if (index !== -1) {
      children.splice(index, 1);
      if (children.length === 0) {
        parent.data.children = undefined;
      }
    }
    dataSource.value = [...dataSource.value];
  } catch (error) {
    console.error('Error removing node:', error);
  }
};

const renderContent = (
  h: (arg0: string, arg1: { class?: string; onClick?: (() => void) | (() => void) | (() => void); style?: string; } | null, arg2: string, arg3: undefined, arg4: undefined) => any,
  {
    node,
    data,
    store,
  }: {
    node: Node;
    data: Tree;
    store: Node["store"];
  }
) => {
  // 安全性处理：转义 node.label
  const safeLabel = node?.label ? node.label.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;') : '';

  // 边界条件处理：检查 node 和 data 是否存在
  if (!node || !data) {
    return h("span", { class: "custom-tree-node" }, "Invalid node or data");
  }

  // 提取 onClick 事件处理函数
  const handleAppend = () => {
    if (typeof append === 'function') {
      append(data);
    } else {
      console.error('append function is not defined');
    }
  };

  const handleRemove = () => {
    if (typeof remove === 'function') {
      remove(node, data);
    } else {
      console.error('remove function is not defined');
    }
  };

  const handleModify = () => {
    if (typeof modify === 'function') {
      modify(node, data);
    } else {
      console.error('modify function is not defined');
    }
  };

  // 使用常量定义样式字符串
  const deleteButtonStyle = "margin-left: 8px";

  return h(
    "span",
    {
      class: "custom-tree-node",
    },
    h("span", null, safeLabel),
    h("span", null, 
      h(
        "a",
        {
          onClick: handleAppend,
        },
        "Append "
      ),
      h(
        "a",
        {
          style: deleteButtonStyle,
          onClick: handleModify,
        },
        "Modify "
      ),
      h(
        "a",
        {
          style: deleteButtonStyle,
          onClick: handleRemove,
        },
        "Delete"
      )
    )
  );
};

const dataSource = ref<Tree[]>([
  {
    id: 1,
    label: "Level one 1",
    children: [
      {
        id: 4,
        label: "Level two 1-1",
        children: [],
      },
    ],
  }
]);
</script>

<style>
.custom-tree-node {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 14px;
  padding-right: 8px;
}
</style>