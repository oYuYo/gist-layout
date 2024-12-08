<script setup lang="ts">
import Header from './components/Header.vue'
import GistItem from './components/GistItem.vue'
</script>

<template>
  <Header />

  <main>
    <div class="wrapper">
      <div class="head">
        <h1>Gist Items</h1>
      </div>
      <Suspense>
        <template #default>
          <GistItem />
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
