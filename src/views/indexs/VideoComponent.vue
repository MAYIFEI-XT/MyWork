<template>
  <div class="mapwrap">
    <dv-border-box-13 class="border-box-13">
      <div class="returnmap" @click="hideVideoAndShowMap">
        返回
      </div>
      <video class="contetn_center_top" controls autoplay muted loop ref="videoPlayer">
        <source :src="videoUrl" type="video/mp4">
      
      </video>
    </dv-border-box-13>
  </div>
</template>

<script>
import { eventBus } from '../../main.js';
import { currentGET } from 'api/modules';

export default {
  name: 'VideoComponent',
  props: {
    videoUrl: {
      type: String,
      default: ''
    }
  },
  data() {
    return {};
  },
  created() {
    console.log('VideoComponent created, videoUrl:', this.videoUrl);
  },
  mounted() {
    console.log('VideoComponent mounted, videoUrl:', this.videoUrl);
  },
  watch: {
    videoUrl(newUrl) {
      if (newUrl) {
        this.$nextTick(() => {
          const videoPlayer = this.$refs.videoPlayer;
          if (videoPlayer) {
            videoPlayer.load();
            videoPlayer.play();
          }
        });
      }
    }
  },
  methods: {
    hideVideoAndShowMap() {
      eventBus.$emit('hide-video-and-show-map');
    }
  }
};
</script>

<style scoped lang="scss">
.mapwrap,
.border-box-13 {
  box-sizing: border-box;
}

.mapwrap {
  height: 548px;
  width: 100%;
  position: relative;
  display: flex;

  justify-content: center;
  align-items: center;

.returnmap {
    position: absolute;
    right: 20px;
    top: -30px;
    width: 80px;
    height: 28px;
    border: 1px solid #00eded;
    border-radius: 10px;
    color: #00f7f6;
    text-align: center;
    line-height: 26px;
    letter-spacing: 6px;
    cursor: pointer;
    box-shadow: 0 2px 4px rgba(0, 237, 237, 0.5), 0 0 6px rgba(0, 237, 237, 0.4);
  }

.border-box-13 {
    width: 100%;
    height: 100%;
 
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
  }

.contetn_center_top {
    width: 100%;
    height: 100%;
	margin: 0 auto;

    video {
      width: 100%;
      height: 100%;
	
      object-fit: cover;
    }
  }
}
</style>