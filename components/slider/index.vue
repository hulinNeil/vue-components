<template>
  <div class="uni-slider"
       ref="uni-slider"
       @click="_onClick">
    <div class="uni-slider-wrapper">
      <div class="uni-slider-tap-area">
        <div
          :style="setBgColor"
          class="uni-slider-handle-wrapper">
          <div
            ref="uni-slider-handle"
            :style="setBlockBg"
            class="uni-slider-handle"/>
          <div
            :style="setBlockStyle"
            class="uni-slider-thumb"/>
          <div
            :style="setActiveColor"
            class="uni-slider-track"/>
        </div>
      </div>
      <span
        v-show="showValue"
        class="uni-slider-value">{{ sliderValue }}</span>
    </div>
    <slot/>
  </div>
</template>
<script>
  import touchtrack from '../util/touchtrack';

  export default {
    name: 'Slider',
    mixins: [touchtrack],
    props: {
      name: {
        type: String,
        default: ''
      },
      min: {
        type: [Number, String],
        default: 0
      },
      max: {
        type: [Number, String],
        default: 100
      },
      value: {
        type: [Number, String],
        default: 0
      },
      step: {
        type: [Number, String],
        default: 1
      },
      disabled: {
        type: [Boolean, String],
        default: false
      },
      color: {
        type: String,
        default: '#e9e9e9'
      },
      backgroundColor: {
        type: String,
        default: '#e9e9e9'
      },
      activeColor: {
        type: String,
        default: '#FFDD00'
      },
      selectedColor: {
        type: String,
        default: '#FFDD00'
      },
      blockColor: {
        type: String,
        default: '#FFDD00'
      },
      blockSize: {
        type: [Number, String],
        default: 10
      },
      showValue: {
        type: [Boolean, String],
        default: false
      }
    },
    data () {
      return {
        sliderValue: Number(this.value)
      };
    },
    computed: {
      setBlockStyle () {
        return {
          width: this.blockSize + 'px',
          height: this.blockSize + 'px',
          marginLeft: -this.blockSize / 2 + 'px',
          marginTop: -this.blockSize / 2 + 'px',
          left: this._getValueWidth(),
          backgroundColor: this.blockColor
        };
      },
      setBgColor () {
        return {
          backgroundColor: this._getBgColor()
        };
      },
      setBlockBg () {
        return {
          left: this._getValueWidth()
        };
      },
      setActiveColor () { // 有问题，设置最大值最小值是有问题
        return {
          backgroundColor: this._getActiveColor(),
          width: this._getValueWidth()
        };
      }
    },
    watch: {
      value (val) {
        this.sliderValue = Number(val);
      }
    },
    mounted () {
      this.touchtrack(this.$refs['uni-slider-handle'], '_onTrack');
    },
    methods: {
      _onUserChangedValue (e) {
        let slider = this.$refs['uni-slider'];
        let offsetWidth = slider.offsetWidth;
        let boxLeft = slider.getBoundingClientRect().left;
        let value = (e.x - boxLeft) * (this.max - this.min) / offsetWidth + Number(this.min);
        this.sliderValue = this._filterValue(value);
      },
      _filterValue (e) {
        return e < this.min ? this.min : e > this.max ? this.max : Math.round((e - this.min) / this
          .step) * this.step + Number(this.min);
      },
      _getValueWidth () {
        return 100 * (this.sliderValue - this.min) / (this.max - this.min) + '%';
      },
      _getBgColor () {
        return this.backgroundColor !== '#e9e9e9' ? this.backgroundColor : (this.color !== '#007aff' ? this.color
          : '#007aff');
      },
      _getActiveColor () {
        return this.activeColor !== '#007aff' ? this.activeColor : (this.selectedColor !== '#e9e9e9' ? this.selectedColor
          : '#e9e9e9');
      },
      _onTrack: function (e) {
        if (!this.disabled) {
          return e.detail.state === 'move' ? (this._onUserChangedValue({
            x: e.detail.x0
          }), this.$emit('changing', {
            value: this.sliderValue,
            type: 'changing'
          })) : void (e.detail.state === 'end' && this.$emit('change', {
            value: this.sliderValue,
            type: 'change'
          }));
        }
      },
      _onClick ($event) {
        if (this.disabled) {
          return;
        }
        this._onUserChangedValue($event);
        this.$emit('change', {
          value: this.sliderValue,
          type: 'change'
        });
      }
    }
  };
</script>
<style scoped>
  .uni-slider {
    margin: 10px 18px;
    padding: 0;
    display: block;
  }

  .uni-slider[hidden] {
    display: none;
  }

  .uni-slider .uni-slider-wrapper {
    display: -webkit-flex;
    display: flex;
    -webkit-align-items: center;
    align-items: center;
    min-height: 16px;
  }

  .uni-slider .uni-slider-tap-area {
    -webkit-flex: 1;
    flex: 1;
    padding: 8px 0;
  }

  .uni-slider .uni-slider-handle-wrapper {
    position: relative;
    height: 4px;
    border-radius: 8px;
    background-color: #e9e9e9;
    cursor: pointer;
    transition: background-color 0.3s ease;
    -webkit-tap-highlight-color: transparent;
  }

  .uni-slider .uni-slider-track {
    height: 100%;
    border-radius: 6px;
    background-color: #007aff;
    transition: background-color 0.3s ease;
  }

  .uni-slider .uni-slider-handle,
  .uni-slider .uni-slider-thumb {
    position: absolute;
    left: 50%;
    top: 50%;
    cursor: pointer;
    border-radius: 50%;
    transition: border-color 0.3s ease;
  }

  .uni-slider .uni-slider-handle {
    width: 28px;
    height: 28px;
    margin-top: -14px;
    margin-left: -14px;
    background-color: transparent;
    z-index: 3;
  }

  .uni-slider .uni-slider-thumb {
    z-index: 2;
    box-shadow: 0 0 4px rgba(0, 0, 0, 0.2);
  }

  .uni-slider .uni-slider-step {
    position: absolute;
    width: 100%;
    height: 2px;
    background: transparent;
    z-index: 1;
  }

  .uni-slider .uni-slider-value {
    color: #888;
    font-size: 14px;
    margin-left: 1em;
  }

  .uni-slider .uni-slider-disabled .uni-slider-track {
    background-color: #ccc;
  }

  .uni-slider .uni-slider-disabled .uni-slider-thumb {
    background-color: #FFF;
    border-color: #ccc;
  }

</style>
