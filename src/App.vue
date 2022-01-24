<template>
  <div class="app">
    <header class="app__header">
      <h1>screen record</h1>
    </header>

    <main class="app__container">
      <div class="app__content">
        <h2 class="app__subtitle">video record</h2>
        <video src="" autoplay class="app__video-feedback"></video>

        <div class="app__actions">
          <button class="app__actions__button">start recording</button>
          <button class="app__actions__button" disabled>stop recording</button>
        </div>
      </div>

      <div v-if="false" class="app__content">
        <h2 class="app__subtitle">Download your video</h2>
        <video src="" autoplay class="app__recorded-video"></video>
        <div class="app__actions">
          <button class="app__actions__button">download video</button>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
};
</script>

<style lang="scss" scoped>
.app {
  $this: &;
  width: 100vw;
  min-height: 100vh;
  background: #1a1a40;
  color: #e0dfdf;

  &__header {
    background: #270082;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 1rem 0;
    text-transform: capitalize;
  }

  #{$this}__container {
    overflow: hidden;
    padding: 2rem;
    display: flex;
    align-items: center;
    flex-direction: column;

    #{$this}__content {
      width: 60vw;
      margin-bottom: 4rem;
      &__subtitle {
        text-transform: uppercase;
      }

      #{$this}__video-feedback,
      #{$this}__recorded-video {
        background: #000000;
        width: 100%;
        height: 30rem;
        margin: 1rem 0;
      }

      #{$this}__actions {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 1rem;

        &__button {
          width: 100%;
          text-transform: uppercase;
          padding: 1rem 2rem;
          font-weight: 700;
          transition: all 300ms;
          background: #fa58b6;
          border: none;
          border-radius: 8px;
          color: #ffffff;
          cursor: pointer;

          &:hover {
            opacity: 0.8;
          }

          &:disabled {
            opacity: 0.4;
            cursor: initial;
          }
        }
      }
    }
  }
}
</style>
