<template>
  <div class="refresh-page">
    <refresh
      v-if="enablePullDownRefresh"
      ref="refresh"
      :color="refreshOptions.color"
      :offset="refreshOptions.offset"
    />
    <div
      class="refresh-body"
      @touchstart="_touchstart"
      @touchmove="_touchmove"
      @touchend="_touchend"
      @touchcancel="_touchend"
    >
      <slot></slot>
    </div>
  </div>
</template>

<script>
import pullToRefresh from "./pull-to-refresh";
import refresh from "./refresh";

export default {
  name: "Refresh",
  mixins: [pullToRefresh],
  components: { refresh },
  props: {
    enablePullDownRefresh: {
      type: Boolean,
      default: true
    },
    pullToRefresh: {
      type: Object,
      default() {
        return {};
      }
    }
  },
  data() {
    const refreshOptions = Object.assign(
      {
        support: true,
        color: "#2BD009",
        style: "circle",
        height: 70,
        range: 150,
        offset: 0
      },
      this.pullToRefresh
    );
    return {
      refreshOptions
    };
  }
};
</script>

<style>
.refresh-body {
  height: 100%;
}
.refresh-page {
  height: 100%;
}
</style>