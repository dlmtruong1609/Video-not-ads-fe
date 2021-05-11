<template>
<div class="bg-dark">
  <div class="container pt-5">
    <div style="width: 100%">
      <video-player :src="src" />
       <h3 style="display: flex; justify-content: space-around;color: white" class="mt-3">
          Nhập tên phim hoặc kèm tập phim: (Conan tập 1000, Conan...)
        </h3>
      <b-input-group class="mt-3" inline>
       
        <b-form-input
          @keyup.enter="onSearch"
          required
          style="width: 50%; margin: auto; border-radius: 15px"
          placeholder="Ví dụ: Conan tập 1000, Onepice tập 200,..."
          v-if="!busy"
          v-model="keyword"
          type="text"
        >
        </b-form-input>
        <template #append>
          <div style="height: 100%; position: relative; cursor: pointer; font-size: 23px; z-index:1000" @click="onSearch">
            <b-icon
              style="
                position: absolute;
                right: 15px;
                top: 50%;
                transform: translateY(-50%);
              "
              icon="search"
            ></b-icon>
          </div>
        </template>
      </b-input-group>

      <div class="mt-3 text-white" v-if="busy">
        Chờ 1 xíu nha, mình đang kiếm cho bạn đây
      </div>
      <div
      class="mt-4"
        style="display: flex; flex-wrap: wrap; justify-content: space-between;"
      >
        <div v-for="(video, index) in videos" :key="index">
          <div class="card movie_card">
            <img :src="video.thumbnail" class="card-img-top" alt="..." />

            <button
              @click="onSetActive(video.url, index)"
              :class="`btn-play-video ${activeIndex === index ? 'active' : ''}`"
            >
              <span
                :class="`play ${activeIndex === index ? 'active' : ''}`"
              ></span>
              <span
                :class="`small-box ${
                  activeIndex === index ? 'active' : 'pauseToPlay'
                }`"
              ></span>
            </button>
            <div class="card-body">
              <h5 class="card-title">{{ video.title }} {{ video.episode ? `tập ${video.episode}` : ''}}</h5>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
import videoPlayer from '@/components/video.player.vue';
import axios from 'axios';
export default {
  components: { videoPlayer },
  data() {
    return {
      keyword: null,
      videos: [],
      busy: false,
      src: '',
      activeIndex: -1,
      host: process.env.VUE_APP_HOST_API,
    };
  },
  methods: {
    async onSearch() {
      console.log(process.env.VUE_APP_HOST_API);
      this.busy = true;
      try {
        var config = {
          method: 'get',
          url: `${this.host}/xemphim?keyword=${this.keyword}`,
          headers: {},
        };

        const response = await axios(config);
        this.videos = response.data;
        console.log(this.videos);
        this.busy = false;
        this.activeIndex = -1
      } catch (error) {
        console.log(error);
        this.busy = false;
      }
    },

    onSetActive(url, index) {
      this.activeIndex = index;
      this.src = url;
    },
  },
};
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
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
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

img {
  -webkit-user-drag: none;
  -moz-user-drag: none;
  -o-user-drag: none;
  user-drag: none;
}
img {
  pointer-events: none;
}

.card {
  border: none !important;
  border-radius: 10px !important;
}
.movie_card {
  padding: 0 !important;
  width: 22rem;
  margin: 14px;
  border-radius: 10px;
  box-shadow: 0 3px 4px 0 rgba(0, 0, 0, 0.2), 0 4px 15px 0 rgba(0, 0, 0, 0.19);
}
.movie_card img {
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  height: 33rem;
}
.movie_info {
  color: #5e5c5c;
}

.movie_info i {
  font-size: 20px;
}
.card-title {
  width: 100%;
}

.card-body {
  display: flex;
  justify-content: space-around;
}


.btn-play-video {
  width: 65px;
  height: 65px;
  border-radius: 50%;
  position: absolute;
  right: 20px;
  bottom: 100px;
  outline: none;
  background: #f64747;
  border: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.btn-play-video::before {
  content: '';
  position: absolute;
  z-index: 10;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  border-radius: 50%;
  border: 3px solid #1f3a93;
  background: #1f3a93;
  transform: scale(0);
  transform-origin: 83% 86%;
  transition-property: transform;
  transition-duration: 0.5s;
  transition-timing-function: ease-out;
}

.btn-play-video.active::before {
  transition-timing-function: ease;
  transform: scale(1);
}

.btn-play-video .play {
  width: 30px;
  height: 30px;
  background: #fff;
  margin-left: 6px;
  z-index: 100;
  clip-path: polygon(0 0, 80% 48%, 0 100%, 0% 100%);
  transition: clip-path 0.5s ease;
  transition-delay: 0.4s;
}

.btn-play-video .play.active {
  transition: clip-path 0.1s;
  clip-path: polygon(0 0, 27% 0, 27% 100%, 0% 100%);
}

.small-box {
  width: 8px;
  height: 15px;
  background: #fff;
  position: absolute;
  left: 65px;
  z-index: 100;
  bottom: 59px;
  opacity: 0;
  transition: opacity 1s;
}

.small-box.pauseToPlay {
  animation: pauseToPlay 0.4s ease alternate-reverse;
}

@keyframes pauseToPlay {
  10% {
    transform: rotate(90deg);
    left: 35px;
  }
  20%,
  40% {
    transform: rotate(180deg);
    left: 35px;
  }
  50% {
    left: 35px;
    height: 29px;
    bottom: 18px;
    transform: rotate(180deg);
  }
  100% {
    left: 35px;
    height: 29px;
    bottom: 18px;
    transform: rotate(180deg);
  }
}

.small-box.active {
  opacity: 1;
  transition: opacity 1s;
  animation: move 0.8s cubic-bezier(0.52, 1.64, 0.37, 0.66) forwards;
  animation-delay: 0.2s;
}

@keyframes move {
  10% {
    transform: rotate(90deg);
    left: 23px;
  }
  20%,
  40% {
    transform: rotate(180deg);
    left: 35px;
  }
  50% {
    left: 35px;
    height: 29px;
    bottom: 18px;
    transform: rotate(180deg);
  }
  100% {
    left: 35px;
    height: 29px;
    bottom: 18px;
    transform: rotate(180deg);
  }
}
</style>
