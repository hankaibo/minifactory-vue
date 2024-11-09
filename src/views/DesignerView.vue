<script setup>
import { ref, onMounted } from 'vue'
import {
  draggable,
  dropTargetForElements,
  monitorForElements,
} from '@atlaskit/pragmatic-drag-and-drop/element/adapter'

const dragging = ref(false)
const state = ref('idle')
const name = ref('')
const attrs = ref({})

const panelData = [
  {
    value: '0',
    header: '基础组件',
    content: [
      { text: '按钮', name: 'button' },
      { text: '分割符', name: 'divider' },
      { text: '悬浮按钮', name: 'fab' },
      { text: '图标', name: 'icon' },
      { text: '布局', name: 'layout' },
      { text: '链接', name: 'link' },
    ],
  },
  {
    value: '1',
    header: '导航',
    content: [
      { text: '返回顶部', name: 'backTop' },
      { text: '抽屉', name: 'drawer' },
      { text: '索引', name: 'indexes' },
      { text: '导航条', name: 'navbar' },
      { text: '侧边栏', name: 'sideBar' },
      { text: '步骤条', name: 'steps' },
      { text: '标签栏', name: 'tabBar' },
      { text: '选项卡', name: 'tabs' },
    ],
  },
  {
    value: '2',
    header: '输入',
    content: [
      { text: '日历', name: 'calendar' },
      { text: '级联选择器', name: 'cascader' },
      { text: '复选框', name: 'checkbox' },
      { text: '时间选择器', name: 'dateTimePicker' },
      { text: '表单', name: 'form' },
      { text: '输入框', name: 'input' },
      { text: '选择器', name: 'picker' },
      { text: '单选框', name: 'radio' },
      { text: '评分', name: 'rate' },
      { text: '搜索框', name: 'search' },
      { text: '滑动选择器', name: 'slider' },
      { text: '步进器', name: 'stepper' },
      { text: '开关', name: 'switch' },
      { text: '多行输入框', name: 'textarea' },
      { text: '树形选择器', name: 'treeSelect' },
      { text: '上传', name: 'upload' },
    ],
  },
  {
    value: '3',
    header: '数据展示',
    content: [
      { text: '头像', name: 'avatar' },
      { text: '徽标', name: 'badge' },
      { text: '单元格', name: 'cell' },
      { text: '折叠面板', name: 'collapse' },
      { text: '倒计时', name: 'countDown' },
      { text: '空状态', name: 'empty' },
      { text: '页脚', name: 'footer' },
      { text: '宫格', name: 'grid' },
      { text: '图片', name: 'image' },
      { text: '图片预览', name: 'imageViewer' },
      { text: '列表', name: 'list' },
      { text: '进度条', name: 'progress' },
      { text: '结果', name: 'result' },
      { text: '骨架屏', name: 'skeleton' },
      { text: '吸顶容器', name: 'sticky' },
      { text: '轮播', name: 'swiper' },
      { text: '表格', name: 'table' },
      { text: '标签', name: 'tag' },
    ],
  },
  {
    value: '4',
    header: '反馈',
    content: [
      { text: '动作面板', name: 'actionSheet' },
      { text: '对话框', name: 'dialog' },
      { text: '下拉菜单', name: 'dropdownMenu' },
      { text: '引导', name: 'guide' },
      { text: '加载中', name: 'loading' },
      { text: '消息', name: 'message' },
      { text: '公告栏', name: 'noticeBar' },
      { text: '遮罩层', name: 'overlay' },
      { text: '弹出气泡', name: 'popover' },
      { text: '弹出层', name: 'popup' },
      { text: '下拉刷新', name: 'pullDownRefresh' },
      { text: '滑动单元格', name: 'swipeCell' },
      { text: '轻提示', name: 'toast' },
    ],
  },
]

const setDragging = (val) => {
  dragging.value = val
}

const setState = (val) => {
  state.value = val
}

onMounted(() => {
  monitorForElements({
    onDrop({ source, a }) {
      console.log('source, a :>> ', source, a)
      name.value = `t-${source.data.name}`
      attrs.value = {
        text: '填充按钮',
      }
    },
  })
})
</script>

<template>
  <div class="container">
    <div class="components">
      <t-collapse>
        <t-collapse-panel
          v-for="panel in panelData"
          :key="panel.value"
          :value="panel.value"
          :header="panel.header"
        >
          <div class="list">
            <div
              class="item"
              :ref="
                (el) =>
                  draggable({
                    element: el,
                    getInitialData: () => ({ ...item }), // NEW
                    onDragStart: () => setDragging(true), // NEW
                    onDrop: () => setDragging(false), // NEW
                  })
              "
              v-for="item in panel.content"
              :key="item"
            >
              {{ item.text }}
            </div>
          </div>
        </t-collapse-panel>
      </t-collapse>
    </div>
    <div
      class="designer"
      :ref="
        (el) =>
          dropTargetForElements({
            element: el,
            getData: () => ({ a: 'a' }),
            canDrop: () => true,
            onDragEnter: ({ source }) => {
              console.log('source :>> ', source)
              setState('valiMove')
            },
            onDragLeave: () => setState('idle'),
            onDrop: () => setState('idle'),
          })
      "
    >
      <div class="phone__header"></div>
      <div class="phone__body">
        <component :is="name" :attrs="attrs">{{ attrs.text }}</component>
      </div>
    </div>
    <div class="config"></div>
  </div>
</template>

<style>
.container {
  display: flex;
  height: 100vh;
}

.components {
  flex-basis: 250px;
  background-color: bisque;
}
.designer {
  width: min(75vw, 375px);
  border-radius: 6px;
  border: 1px solid #e7e7e7;
  outline: transparent solid 9999px;
  display: block;
}
.phone__header {
  height: var(--phone-header-height);
  padding: 8px;
  border-radius: 6px 6px 0px 0px;
  box-sizing: border-box;
  background: var(--bg-color-demo);
}
.phone__body {
  width: 100%;
  height: var(--phone-body-height);
  border-radius: 0px 0px 6px 6px;
  background-color: var(--bg-color-demo);
}

.config {
  flex-basis: 250px;
}

.list {
  display: flex;
  flex-direction: column;
  max-width: 300px;
  margin: 0 auto;
}
.item {
  padding: 10px;
  margin: 5px;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  cursor: grab;
}
.dragging {
  opacity: 0.5;
}
</style>
