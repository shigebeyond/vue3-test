<script setup>
import { ref, computed, watch, watchEffect, onMounted, onUnmounted, onUpdated } from 'vue'
import HelloWorld from './components/HelloWorld.vue'

// 响应式状态
const count = ref(0)
const attr = 'value'
// 一个计算属性 ref
const evenDesc = computed(() => {
  return count.value % 2 == 0 ? '偶数' : '奇数'
})
const isActive = ref(false)
const activeClass = ref('')
const activeColor = ref('green')
const fontSize = ref(30)
const baseStyles = ref({
  color: 'yellow',
  fontSize: '23px'
})
const items = ref([
  { message: 'Foo' }, 
  { message: 'Bar' }
])
const myObject = ref({
  title: 'How to do lists in Vue',
  author: 'Jane Doe',
  publishedAt: '2016-04-10'
})
const text = ref('')
const message = ref('')
const checked = ref(false)
const checkedNames = ref([])
const picked = ref(null)
const selected = ref(null)
const options = ref([
  { text: 'One', value: 'A' },
  { text: 'Two', value: 'B' },
  { text: 'Three', value: 'C' }
])

// 子组件按钮放大的字体大小
const postFontSize = ref(1)

// 用来修改状态、触发更新的函数
function increment() {
  count.value++
  let even = count.value % 2 == 0
  isActive.value = even
  activeClass.value = even ? 'active2' : ''
}

// 声明一个 ref 来存放该元素的引用
// 必须和模板里的 ref 同名
const input = ref(null)

// 生命周期钩子
// https://cn.vuejs.org/api/composition-api-lifecycle.html#onmounted
// 监听组件挂载完
onMounted(() => {
  console.log(`初始化处理: ${count.value}.`)
  // 引用组件
  //input.value.focus()
})

// 监听组件卸载
onUnmounted(() => {
  console.log(`卸载处理: ${count.value}.`)
})

// 监听状态变更导致的dom更新完
onUpdated(() => {
  // 文本内容应该与当前的 `count.value` 一致
  console.log(document.getElementById('count').textContent)
})

// 准备侦听的状态
const question = ref('')
const answer = ref('')
// 可以直接侦听一个状态(ref)
watch(question, async (newQuestion, oldQuestion) => {
  answer.value = `<a target="_blank" href='https://www.baidu.com/s?ie=UTF-8&wd=${newQuestion}'>搜索问题</a>`
})
// 监听所有状态
/*watchEffect(async () => {
  if(question.value != '')
    answer.value = `<a target="_blank" href='https://www.baidu.com/s?ie=UTF-8&wd=${question.value}'>搜索问题</a>`
})*/

</script>

<template>  
  点击按钮加1:
  <button id="count" @click="increment">{{ count }}</button>
  <br>
  属性绑定:
  <input type="text" name="c" :value="count">
  <br>
  动态属性绑定:
  <input type="text" name="d" :[attr]="count">
  <br>
  if指令:
  <span>
    <p v-if="count % 2 == 0">偶数</p>
    <p v-else>奇数</p>
    <p v-show="count % 2 == 0">😢</p>
  </span>
  <br>
  计算属性:
  {{count % 2 === 0 ? '偶数' : '奇数' }}
  <br>
  计算属性2:
  {{ evenDesc }}
  <br>
  绑定class-绑对象:
  <div
    class="static"
    :class="{ active: isActive}"
  ></div>
  <br>
  绑定class-绑数组:
  <div :class="[activeClass, ]"></div>
  <br>
  绑定内联样式-绑对象:
  <div :style="{ color: activeColor, fontSize: fontSize + 'px' }">hello</div>
  <br>
  绑定内联样式-绑数组:
  <div :style="[baseStyles, ]">world</div>
  <br>
  for数组：
  <span>
    <li v-for="item in items">
      {{ item.message }}
    </li>
  </span>
  <br>
  for数组(带下标)：
  <span>
    <li v-for="(item, index) in items" :key="index">
      {{index}} -- {{ item.message }}
    </li>
  </span>
  <br>
  for对象：
  <span>
    <li v-for="(value, key) in myObject">
      {{ key }}: {{ value }}
    </li>
  </span>
  <br>
  for范围：
  <span>
    <p v-for="n in 10">{{ n }}</p>
  </span>
  <br>
  表单输入绑定input:
  <span>
    <!-- 等价 <input :value="text" @input="event => text = event.target.value"> -->
    <input v-model="text">
    {{text}}
  </span>
  <br>
  表单输入绑定textarea:
  <span>
    <textarea v-model="message" placeholder="add multiple lines"></textarea>
    {{message}}
  </span>
  <br>
  表单输入绑定checkbox:
  <span>
    <input type="checkbox" id="checkbox" v-model="checked" />
    <label for="checkbox">{{ checked }}</label>
  </span>
  <br>
  表单输入绑定checkbox组:
  <span>
    <div>Checked names: {{ checkedNames }}</div>

    <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
    <label for="jack">Jack</label>

    <input type="checkbox" id="john" value="John" v-model="checkedNames">
    <label for="john">John</label>

    <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
    <label for="mike">Mike</label>
  </span>
    <br>
  表单输入绑定radio组:
  <span>
    <div>Picked: {{ picked }}</div>

    <input type="radio" id="one" value="One" v-model="picked" />
    <label for="one">One</label>

    <input type="radio" id="two" value="Two" v-model="picked" />
    <label for="two">Two</label>
  </span>
      <br>
  表单输入绑定select:
  <span>
    <div>Selected: {{ selected }}</div>

    <!-- <select v-model="selected">
      <option disabled value="">Please select one</option>
      <option>A</option>
      <option>B</option>
      <option>C</option>
    </select> -->
    <select v-model="selected">
      <option v-for="option in options" :value="option.value">
        {{ option.text }}
      </option>
    </select>
  </span>
  <br>
  引用控件:
   <input ref="input" />
  <br>
  状态侦听:
  <span>
     <p>
      提问:
      <input v-model="question" />
    </p>
    <p>
      答案：<span v-html="answer"></span>
    </p>
  </span>
  <br>
   组件:
   <div :style="{ fontSize: postFontSize + 'em' }">
      <!-- 子组件按钮触发事件enlarge-text，父组件通过 @enlarge-text 来捕获 -->
     <HelloWorld msg="一站云" @enlarge-text="postFontSize += 0.1">
      通过插槽来分配内容
     </HelloWorld>
   </div>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.active {
  background-color: red;
}
.active2 {
  background-color: blue;
}

</style>
