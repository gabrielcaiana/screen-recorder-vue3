<template>
  <div class="app">
    <header class="app__header">
      <h1>screen record</h1>
    </header>

    <main class="app__container">
      <div class="app__content">
        <h2 class="app__subtitle">video record</h2>
        <video
          src=""
          ref="videoFeedback"
          autoplay
          class="app__video-feedback"
        ></video>

        <div class="app__actions">
          <button
            @click="startRecording"
            :disabled="startDisabled"
            class="app__actions__button"
          >
            start recording
          </button>
          <button
            @click="stopRecording"
            url="https://www.gabrielcaiana.com"
            class="app__actions__button"
            :disabled="stopDisabled"
          >
            stop recording
          </button>
        </div>
      </div>

      <div id="recordedVideoWrap" v-show="downloadMode" class="app__content">
        <h2 class="app__subtitle">Download your video</h2>
        <video src="" ref="videoRecorded" class="app__recorded-video"></video>
        <div class="app__actions">
          <a href="" ref="downloadButton" class="app__actions__button">
            download video
          </a>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',

  data() {
    return {
      stream: null,
      audio: null,
      mixedStream: null,
      chunks: [],
      recorder: null,
      startDisabled: false,
      stopDisabled: true,
      downloadMode: false,
    };
  },

  methods: {
    async setupStream() {
      try {
        this.stream = await navigator.mediaDevices.getDisplayMedia({
          video: true,
        });

        this.audio = await navigator.mediaDevices.getUserMedia({
          audio: {
            echoCancellation: true,
            noiseSuppression: true,
            sampleRate: 44100,
          },
        });

        this.setupVideoFeedback();
      } catch (err) {
        console.log(err);
      }
    },

    setupVideoFeedback() {
      if (this.stream) {
        const video = this.$refs.videoFeedback;
        video.srcObject = this.stream;
        video.play();
      } else {
        console.log(`No stream avaliable!`);
      }
    },

    async startRecording() {
      await this.setupStream();

      if (this.stream && this.audio) {
        this.mixedStream = new MediaStream([
          ...this.stream.getTracks(),
          ...this.audio.getTracks(),
        ]);

        this.recorder = new MediaRecorder(this.mixedStream);
        this.recorder.ondataavailable = this.handleDataAvaliable;
        this.recorder.onstop = this.handleStop;
        this.recorder.start(200);

        this.startDisabled = true;
        this.stopDisabled = false;

        console.log('Recording has stared...');
      } else {
        console.warn('No stream avaliable');
      }
    },

    stopRecording() {
      this.recorder.stop();
      this.startDisabled = false;
      this.stopDisabled = true;

      console.log('Recording has stopped...');
    },

    handleDataAvaliable(e) {
      this.chunks.push(e.data);
    },

    handleStop(e) {
      const blob = new Blob(this.chunks, { type: 'video/mp4' });
      this.chunks = [];

      const downloadButton = this.$refs.downloadButton

      downloadButton.href = URL.createObjectURL(blob);
      downloadButton.download = 'video.mp4';

      const recordedVideo = this.$refs.videoRecorded;

      this.downloadMode = true;

      recordedVideo.src = URL.createObjectURL(blob);
      recordedVideo.load();
      recordedVideo.onloadeddata = function () {
        const rc = document.getElementById('recordedVideoWrap');
        rc.scrollIntoView({ behavior: 'smooth', block: 'start' });

        recordedVideo.play();
      };

      this.stream.getTracks().forEach((track) => track.stop());
      this.audio.getTracks().forEach((track) => track.stop());

      console.log('Recording stopped');
    },
  },
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
      width: 50%;
      margin-bottom: 4rem;

      @media (max-width: 900px) {
        width: 80%;
      }

      &__subtitle {
        text-transform: uppercase;
      }

      #{$this}__video-feedback,
      #{$this}__recorded-video {
        background: #000000;
        width: 100%;
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
          text-align: center;
          text-decoration: none;

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
