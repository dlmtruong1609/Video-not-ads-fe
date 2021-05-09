<template>
  <div>
    <video ref="videoPlayer" :key="key" class="video-js vjs-16-9"></video>
  </div>
</template>

<script>
import videojs from 'video.js'

export default {
  name: 'VideoPlayer',
  props: {
    options: {
      type: Object,
      default() {
        return {}
      },
    },

    sources: {
      type: Array,
      default: () => [],
    },
  },

  data() {
    return {
      player: null,
      key: false,
    }
  },

  watch: {
    sources(val) {
      if (val[0]?.src) this.player.src(val)
    },
  },

  mounted() {
    this.initPlayer()
  },

  beforeDestroy() {
    this.destroy()
  },

  methods: {
    initPlayer() {
      const options = {
        ...this.options,
        sources: this.sources,
      }

      this.player = videojs(
        this.$refs.videoPlayer,
        options,
        function onPlayerReady() {}
      )
    },

    destroy() {
      if (this.player) {
        this.player.dispose()
      }
    },

    async sleep(milliseconds) {
      await new Promise((resolve) => setTimeout(resolve, milliseconds)) // 3 sec
    },
  },
}
</script>
