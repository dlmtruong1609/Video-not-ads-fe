<template>
  <div class="container">
    <div>
      <video-player :src="src" />
      <h1 class="mt-4">Xem phim không quảng cáo</h1>
      <h1>Nhập tên phim và tập phim</h1>
      <p>Ví dụ: Conan tập 1000, Onepice tập 200,...</p>
      <b-form-input v-if="!busy" v-model="keyword" type="text"></b-form-input>
       <b-overlay
        :show="busy"
        rounded
        opacity="0.6"
        spinner-small
        spinner-variant="primary"
        class="d-inline-block"
      >
       <b-button :disabled="busy" @click="onSearch" variant="success mt-3">Tìm ngay</b-button>
      </b-overlay>
     
     <div class="mt-3" v-if="busy">Chờ 1 xíu nha, mình đang kiếm cho bạn đây</div>
     <div v-if="urls.length > 0">
       <span v-for="(url, index) in urls" :key="index">
        <b-badge style="color: black; cursor: pointer;font-size: 20px" class="mt-5 mr-3" variant="info" @click="src = url">Link {{ index + 1}}</b-badge>
       </span>
     </div>
    </div>
  </div>
</template>

<script>
import videoPlayer from '@/components/video.player.vue'
import axios from 'axios'
export default {
  components: { videoPlayer },
  data() {
    return {
      keyword: null,
      urls: [],
      busy: false,
      src: ''
    }
  },
  methods: {
    async onSearch() {
      this.busy = true
      try {
        var config = {
          method: 'get',
          url: `http://localhost:3000/xemphim?keyword=${this.keyword}`,
          headers: { }
        };

        const response = await axios(config)
        this.urls = response.data.urls
        console.log(this.urls.length);
        this.busy = false
      } catch (error) {
        console.log(error);
        this.busy = false
      }
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
