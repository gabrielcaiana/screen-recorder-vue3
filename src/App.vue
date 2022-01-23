<template>
  <div class="app">
    <header class="app__header">
      <h1>Screen Record</h1>
    </header>
  </div>
</template>

<script>
  export default {
    name: 'App'
  }
</script>

<style lang="scss" scoped>
.app {
  width: 100vw;
  height: 100vh;
  background: #1A1A40;

  &__header {
    background: #270082;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 1rem 0;
    color: #e0dfdf;
  }
}
</style>