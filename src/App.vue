<template>
  <div class="container">
    <div style="width: 100%;">
      <video-player :src="src" />
      <b-form inline  v-on:submit.prevent="onSearch">
        <h3>Nhập tên phim và tập phim</h3>
        <b-form-input required style="width: 50%; margin: auto;" placeholder="Ví dụ: Conan tập 1000, Onepice tập 200,..." v-if="!busy" v-model="keyword" type="text"></b-form-input>
        <div v-if="urls.length > 0">
        <span v-for="(url, index) in urls" :key="url+index">
          <b-badge style="color: black; cursor: pointer;font-size: 20px" class="mt-2 mr-3" variant="info" @click="src = url">Link {{ index + 1}}</b-badge>
        </span>
        </div>
      </b-form>
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
      src: '',
      host: process.env.VUE_APP_HOST_API
    }
  },
  methods: {
    async onSearch() {
      console.log(process.env.VUE_APP_HOST_API);
      this.busy = true
      try {
        var config = {
          method: 'get',
          url: `${this.host}/xemphim?keyword=${this.keyword}`,
          headers: { }
        };

        const response = await axios(config)
        this.urls = response.data.urls
        console.log(this.urls);
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
