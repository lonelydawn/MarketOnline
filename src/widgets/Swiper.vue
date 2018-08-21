<template>
  <div class="swiper-container">
    <div
      :style="{backgroundImage: 'url(' + bg + ')'}"
      class="swiper-image"></div>
    <div class="swiper-paginator">
      <span
        v-for="(slide, index) in slides"
        :key="index"
        class="paginator-item"
        :class="{'paginator-current': index === value}"
        @mouseover="toggleIndex(index)"
        @mouseout="initTimer"></span>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Swiper',
    props: {
      slides: {
        type: Array,
        validator (value) {
          return value.every(item => Object.prototype.toString.call(item) === '[object String]')
        }
      },
      interval: {
        type: Number,
        default: 4
      },
      value: {
        type: Number,
        default: 0
      }
    },
    data () {
      return {
        timer: null
      }
    },
    computed: {
      bg () {
        return this.slides[this.value]
      }
    },
    mounted () {
      this.$nextTick(function () {
        this.initTimer()
      })
    },
    methods: {
      initTimer () {
        this.timer = setInterval(() => {
          this.$emit('input', (this.value + 1) % this.slides.length) // 自定义v-model
        }, this.interval * 1000)
      },
      toggleIndex (index) {
        this.$emit('input', index)
        clearInterval(this.timer)
      }
    }
  }
</script>

<style scoped>
  .swiper-container {
    position: relative;
    width: 100%;
    height: 100%;
  }
  .swiper-image {
    height: 100%;
    background-size: cover;
    background-repeat: no-repeat;
  }
  .swiper-paginator {
    margin-top: -30px;
    padding-right: 5px;
    text-align: right;
    list-style: none;
  }
  .paginator-item {
    cursor: pointer;
    display: inline-block;
    width: 16px;
    height: 16px;
    margin-left: 5px;
    border-radius: 50%;
    background-color: #000;
    opacity: 0.3;
  }
  .paginator-current {
    background-color: #fff;
    opacity: .6;
  }
</style>
