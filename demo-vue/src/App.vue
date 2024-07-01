<script setup>
import { ref, watch } from 'vue';
// import list from '@/components/list';
// import Computed from './components/computeds.vue';
// import baseVue from './components/base.vue';
// import HelloWorld from './components/HelloWorld.vue';
// import formModel from './components/formModel.vue';
// import life from './components/life.vue';
import { storeToRefs } from 'pinia';
import { useUserStore } from '@/store/user';

const userStore = useUserStore();
// 此时userStore 并不是响应式数据 需要变成响应式的
const { name } = storeToRefs(userStore);
// 方法不需要是响应式的 storeToRefs 不会去处理方法 需要去userStore结构
const { updateName } = userStore;

const type = ref('base');
const message = ref('你好👋');

//鼠标坐标获取
import { useMouse } from './utils/hooks';

const {x,y} = useMouse();   //解构赋值相当于{x.value,y.value}

watch ([x,() => y.value], ([newX , newY]) => {
  console.log(`x is ${newX} and y is ${newY}`);
  },{immediate:true});

const userPath = ref('/user/456');

function warn(message, event) {
  console.log('event', event);
  // 这里可以访问原生事件
  if (event) {
    event.preventDefault();
  }
  alert(message);
}
</script>

<template>
  <div class="h-12 md:ml-10 md:pr-4 md:space-x-8 flex justify-center">
    <div class="nav-wrapper">
      <h2>滚轮下滑↓&nbsp;</h2>
      <div
        class="mr-10"
        :class="{active: type === item}"
        v-for="(item, index) in ['base', 'computed', 'list', 'hello', 'form', 'life', 'fa', 'index']"
        :key="`nav_${index}`"
        @click="type = item"
      >
        <routerLink :to="`/${item}`">{{item}}</routerLink>
      </div>
      <button :class="{ active: type === 'user' }" @click="type = 'user'">
    <router-link :to="userPath">user</router-link>
  </button>
    </div>
  </div>

  <main>
    <router-view />
  </main>
</template>

<style scoped>
.active {
  color: #ff99cc;
}

.border-black {
  border: #000001 solid px;
}

.pd-20 {
  padding: 20px;
}

.mr-10 {
  display: inline-block;
  margin-right: 10px;
}
.nav-wrapper {
  height: 64px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  border-bottom: 1px solid rgba(0, 0, 0, 0.4);
}
</style>
