<template>
  <div>
    <videojs :options="videoOptions" :sources="sources" />
  </div>
</template>

<script>
import 'video.js/dist/video-js.css'
import videojs from '@/components/videojs.vue'

export default {
  name: 'VideoPlayer',
  components: {
    videojs,
  },

  props: {
    src: {
      type: String,
      default: null,
    },
    type: {
      type: String,
      default: 'application/x-mpegURL',
    },
  },

  data() {
    return {
      sources: [],
      videoOptions: {
        autoplay: true,
        controls: true,
        preload: 'true',
        playbackRates: [0.5, 1, 1.5, 2, 4],
      },
    }
  },

  watch: {
    src() {
      if (this.src && this.type) {
        this.sources = []
        this.sources.push({
          src: this.src,
          type: this.type,
        })
      }
    },
  },

  mounted() {
    if (this.src && this.type) {
      this.sources.push({
        src: this.src,
        type: this.type,
      })
    }
  },
}
</script>
<style scoped>
* >>> .video-js {
  width: 100%;
  height: none !important;
}
</style>
