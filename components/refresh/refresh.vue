<template>
  <div ref="refresh" class="refresh-content">
    <div :style="{'margin-top':offset+'px'}" class="refresh">
      <div class="refresh-inner">
        <svg
          :fill="color"
          class="refresh__icon"
          width="24"
          height="24"
          viewBox="0 0 24 24"
        >
          <path
            d="M17.65 6.35C16.2 4.9 14.21 4 12 4c-4.42 0-7.99 3.58-7.99 8s3.57 8 7.99 8c3.73 0 6.84-2.55 7.73-6h-2.08c-.82 2.33-3.04 4-5.65 4-3.31 0-6-2.69-6-6s2.69-6 6-6c1.66 0 3.14.69 4.22 1.78L13 11h7V4l-2.35 2.35z"
          ></path>
          <path d="M0 0h24v24H0z" fill="none"></path>
        </svg>
        <svg class="refresh__spinner" width="24" height="24" viewBox="25 25 50 50">
          <circle
            :stroke="color"
            class="refresh__path"
            cx="50"
            cy="50"
            r="20"
            fill="none"
            stroke-width="4"
            stroke-miterlimit="10"
          ></circle>
        </svg>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "pull-refresh",
  props: {
    color: {
      type: String,
      default: "#2BD009"
    },
    offset: {
      type: Number,
      default: 0
    }
  }
};
</script>

<style>
.refresh-page {
  height: 100%;
}
.refresh-content {
  position: absolute;
  top: 0;
  width: 100%;
  height: 40px;
  display: block;
  box-sizing: border-box;
}

.refresh-content .refresh {
  position: absolute;
  top: -45px;
  left: 50%;
  transform: translate3d(-50%, 0, 0);
  width: 40px;
  height: 40px;
  justify-content: center;
  align-items: center;
  background: #fff;
  border-radius: 50%;
  box-shadow: 0 1px 6px rgba(0, 0, 0, 0.117647),
    0 1px 4px rgba(0, 0, 0, 0.117647);
  display: none;
  z-index: 997;
}

.refresh-content .refresh-inner {
  display: flex;
  align-items: center;
  justify-content: center;
  line-height: 0;
  width: 40px;
  height: 40px;
  border-radius: 50%;
}

.refresh-content.refresh--pulling .refresh,
.refresh-content.refresh--aborting .refresh,
.refresh-content.refresh--reached .refresh,
.refresh-content.refresh--refreshing .refresh,
.refresh-content.refresh--restoring .refresh {
  display: flex;
}

.refresh-content.refresh--pulling .refresh__spinner,
.refresh-content.refresh--aborting .refresh__spinner,
.refresh-content.refresh--reached .refresh__spinner,
.refresh-content.refresh--refreshing .refresh__icon,
.refresh-content.refresh--restoring .refresh__icon {
  display: none;
}

.refresh-content.refresh--refreshing .refresh__spinner {
  transform-origin: center center;
  animation: refresh-rotate 2s linear infinite;
}

.refresh-content.refresh--refreshing .refresh__path {
  stroke-dasharray: 1, 200;
  stroke-dashoffset: 0;
  stroke-linecap: round;
  animation: refresh-dash 1.5s ease-in-out infinite,
    refresh-colorful 6s ease-in-out infinite;
}

@keyframes refresh-rotate {
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}

@keyframes refresh-dash {
  0% {
    stroke-dasharray: 1, 200;
    stroke-dashoffset: 0;
  }

  50% {
    stroke-dasharray: 89, 200;
    stroke-dashoffset: -35px;
  }

  100% {
    stroke-dasharray: 89, 200;
    stroke-dashoffset: -124px;
  }
}
</style>