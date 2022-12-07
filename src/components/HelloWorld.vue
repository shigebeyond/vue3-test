<script setup>
// 1 声明属性，父容器中渲染组件时直接通过属性来传递值
// 1.1 对象方式
defineProps({
  msg: {
    type: String,
    required: true
  }
})

// 1.2 数组方式
// defineProps(['title'])

// 2 声明事件
defineEmits(['enlarge-text'])
</script>

<template>
  <div class="greetings">
    <!-- 输出父组件传递过来的属性 -->
    <h1 class="green">{{ msg }}</h1>
    <!-- 触发事件enlarge-text，父组件通过 @enlarge-text 来捕获 -->
    <button @click="$emit('enlarge-text')">放大字体</button>
    <!-- 通过插槽来分配内容 -->
    <slot />
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
