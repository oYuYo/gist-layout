<script setup lang="ts">
import Header from './components/PageHeader.vue'
import GistItem from './components/GistItem.vue'

import { ref, onMounted,onUnmounted } from 'vue';
const keyword = ref('');

const SetKeyword = (e: KeyboardEvent) => {
  if (e.key === 'Enter') {
    var el = document.querySelector('input[name="search"]') as HTMLInputElement;
    if (el) {
      keyword.value = el.value.trim();
      if (keyword.value) {
        console.log(`Searching for: ${keyword.value}`);
      }
    }
  }
}
onMounted(() => {
  addEventListener('keydown', SetKeyword);
});
onUnmounted(() => {
  removeEventListener('keydown', SetKeyword);
})
</script>

<template>
  <Header />

  <main>
    <div class="wrapper">
      <div class="head search">
        <h1>Gist Items</h1>
        <input type="search" name="search" placeholder="キーワードを入力" />
      </div>
      <Suspense>
        <template #default>
          <GistItem :keyword="keyword"/>
        </template>
        <template #fallback>
          Loading...
        </template>
    </Suspense>
    </div>
  </main>
</template>
<style scoped>
header {
  line-height: 1.5;
}
h1 {
  margin:1rem 0 0.1rem 0;
}
main {
  max-width: 1280px;
  min-width: 375px;
  margin: 0 auto;
}
div.head {
  border-bottom: 1px solid #cccccc;
  overflow-wrap: break-word;
}
div.search {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
}
div.search input {
  padding: 0.5rem;
  border: 1px solid #cccccc;
  border-radius: 4px;
  width: 250px;
}
/*
@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
*/
</style>
