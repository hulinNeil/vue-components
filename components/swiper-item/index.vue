<template>
  <div class="uni-swiper-item">
    <slot />
  </div>
</template>
<script>
  export default {
    name: 'SwiperItem',
    props: {
      itemId: {
        type: String,
        default: ''
      }
    },
    mounted: function() {
      var $el = this.$el
      $el.style.position = 'absolute'
      $el.style.width = '100%'
      $el.style.height = '100%'
      var callbacks = this.$vnode._callbacks
      if (callbacks) {
        callbacks.forEach(callback => {
          callback()
        })
      }
    }
  }
</script>
<style>
  .uni-swiper-item {
    display: block;
    overflow: hidden;
    will-change: transform;
    position: absolute;
    width: 100%;
    height: 100%;
  }

  .uni-swiper-item[hidden] {
    display: none;
  }
</style>
