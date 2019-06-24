<template>
  <div>
    <swiper id="swiper" v-if="showView" @change="change" :current="index">
      <swiper-item v-for="(item,key) in list" :key="key">
        <div class="preview_wraper" @click.stop="close">
          <img :src="item" alt="" class="img" @load="load">
        </div>
      </swiper-item>
    </swiper>
    <span class='preview_dot'>{{index + 1}}/{{list.length}}</span>
  </div>
</template>

<script>
  import Swiper from '../swiper';
  import SwiperItem from '../swiper-item';
  export default {
    name: 'ImagePreview',
    components: {
      SwiperItem,
      Swiper
    },
    data() {
      return {
        list: [],
        index: 0,
        showView: false
      }
    },
    methods: {
      change(e) {
        this.index = e.current;
      },
      show({
        list,
        index
      }) {
        this.list = list;
        this.index = index;
        this.showView = true;
      },
      close() {
        this.list = [];
        this.index = 0;
        this.showView = false;
      },
      load(event) {
        let img = event.path[0];
        let imgHeight = img.naturalHeight,
          imgWidth = img.naturalWidth;
        let proportion = imgHeight / imgWidth;
        if (imgWidth > window.innerWidth) {
          imgWidth = window.innerWidth;
          imgHeight = imgWidth * proportion;
        }
        if (imgHeight > window.innerHeight) {
          imgHeight = window.innerHeight;
          imgWidth = imgHeight / proportion;
        }
        img.style.cssText = "width:" + imgWidth + "px;height:" + imgHeight + "px;";
      }
    }
  }
</script>

<style scoped="scoped">
  #swiper {
    position: fixed;
    z-index: 999;
    background: rgba(0, 0, 0, 0.5);
    height: 100vh !important;
    top: 0;
    left: 0;
    right: 0;
  }

  .preview_wraper {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .img {
    width: 0;
    height: 0;
  }

  .preview_dot {
    position: absolute;
    z-index: 1000;
    bottom: 60px;
    left: 0;
    color: #fff;
    width: 100%;
    text-align: center;
    font-size: 14px;
    letter-spacing: 1px;
  }
</style>
