<!-- 
標準的vue寫法:
<script setup></script> 
<templete></templete>
<style scope></style>

語法範例1:
  嵌入純文字:   {{ 變數名稱 }}
  嵌入屬性:     <div v-bing:屬性名稱="變數名稱">內文</div>
                <div :屬性名稱="變數名稱">內文</div>
  嵌入HTML標籤: <div v-html="變數名稱">

  常用變化:
    文字變換大寫 {{ 變數名稱.toUpperCase() }}
    搭配三元運算  :color="布林值變數?'red':'green'"

語法範例2:
  判斷式: <div v-if="變數名稱 >=< 2000"></div>
          <div v-else-if="判斷式"></div>
          <div v-else></div>

  迴圈:   <div v-for="變數 in 陣列變數">陣列中有 {{變數}}</div>
          <div v-for="(變數, 序數) in 陣列變數">陣列中第{{序數}}個是{{變數}}</div>
          <div v-for="(value, key) in 物件變數">物件中有{{key}}的值是{{value}}</div>

語法範例3:
  事件處理:  <button v-on:click="方法變數"></button>
            <button @click="方法變數"></button>
            <button @mouseover="方法變數"></button>
    修飾字:  <button @click.once="方法變數"></button> 僅觸發一次
            <a @click.prevent="方法變數" href="網址"></a> 停止預設行為(a 標籤點擊不會導向)

語法範例4:
  響應式: 
  <script set up>
    let 變數名稱 = ref("內容");
    let 方法名稱 = function(){變數名稱.value =  "新內容"};
  </script>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
  <templete>
   <div @click="方法名稱">{{ 變數名稱 }}</div>
  </templete>

  輸入元件響應:
  <script set up>
    let 變數名稱 = ref("內容");
    let gender = ref(null);
    let array = ref([]);
  </script>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
  <templete>
    <input type="text" v-model="變數名稱"></input>
    <span> {{變數名稱}} </span>

    <input type="radio" value="F" v-model="gender">
    <input type="radio" value="M" v-model="gender">
    <span> 性別: {{gender}} </span>

    <input type="checkbox" value="A" v-model="array">
    <input type="checkbox" value="B" v-model="array">
    <input type="checkbox" value="C" v-model="array">
    <span> 選擇的有: {{array}} </span>

    <select v-model="gender">
      <option value="" selected disabled>請選擇</option>
      <option value="M">男</option>
      <option value="F">女</option>
    </select>
    <span> 性別: {{gender}} </span>

  </templete>

  元件之間的傳遞:
  父元件標籤給予屬性，子元件的script中以 defineProps(["屬性名稱"])
  
  父元件.vue
  <script setup>
    import 子元件 from '子元件.vue'
  </script>
  <templete>
    <子元件 屬性名稱 = "屬性">
  </templete>

  子元件.vue
  <script setup>
    defineProps(["屬性名稱"])
  </script>
  <templete>
    <div> 傳遞進來的屬性為 {{ 屬性名稱 }} </div>
  </templete>


  自訂事件傳遞
  在子元件中使用 "$emit('自訂事件名稱')"觸發父元件事件

  父元件.vue
  <script setup>
    import 子元件 from '子元件.vue';
    let 函式名稱 = function(){};
  </script>
  <templete>
    <子元件 @自訂事件名稱 = "函式名稱">
  </templete>

  子元件.vue
  <templete>
    <button @click="$emit('自訂事件名稱')"> 點擊傳遞事件給父元素 </button>
  </templete>
  或是
  <script setup>
    let emit = defineEmits(["自訂事件名稱"])
    let 函式名稱 = function(){emit("自訂事件名稱")};
  </script>
  <templete>
    <button @click="函式名稱"> 點擊傳遞事件給父元素 </button>
  </templete>

  常用生命週期
  onMounted()
  onUpdated()
  onUnmounted()
-->
<template>


  <Suspense>
    <template #default>
      <AsyncWelcomeWehelp />
    </template>
    <template #fallback>
      <div class="loading-overlay">
        <div class="spinner"></div>
        <p>載入中...</p>
      </div>
    </template>
  </Suspense>

</template>

<script setup>
import { defineAsyncComponent } from "vue";


const AsyncWelcomeWehelp = defineAsyncComponent(() =>
  import("./views/WelcomeWehelp.vue")
);


</script>

<style>
:root {
  --bg-light: #f1f5f9;
  --text-light: #0f1724;
  --subtext-light: #475569;
  --card-bg-light: #ffffff;

  --bg-dark: #020617;
  --text-dark: #e6eef8;
  --subtext-dark: #cbd5e1;
  --card-bg-dark: #071226;
}



.loading-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.8);
  /* 淺色模式半透明背景 */
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  z-index: 2000;
  /* 確保蓋在最上面 */
  backdrop-filter: blur(4px);
  /* 微模糊背景 */
  transition: opacity 0.3s ease;
}

.mode-dark .loading-overlay {
  background: rgba(0, 0, 0, 0.6);
  /* 深色模式背景 */
}

.spinner {
  width: 60px;
  height: 60px;
  border: 6px solid rgba(0, 0, 0, 0.1);
  border-top: 6px solid #3eaf7c;
  /* 亮色綠藍感 (可改成主題色) */
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 1rem;
}

.mode-dark .spinner {
  border: 6px solid rgba(255, 255, 255, 0.1);
  border-top: 6px solid #4ecdc4;
  /* 深色模式用清爽藍綠 */
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.loading-overlay p {
  font-family: Inter, "Noto Sans TC", sans-serif;
  font-size: 1.1rem;
  font-weight: 500;
  color: #333;
  letter-spacing: 1px;
}

.mode-dark .loading-overlay p {
  color: #eee;
}

/* Reset & Base */
* {
  box-sizing: border-box;
}

body,
html,
#app {
  height: 100%;
  margin: 0;
  font-family: Inter, ui-sans-serif, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
  background-color: var(--bg-light);
  color: var(--text-light);
  transition: background 0.5s, color 0.5s;
}
</style>
