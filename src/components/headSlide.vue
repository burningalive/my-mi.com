<style scoped lang="scss">
  $blue: #49c8fd;
  $s-width: 1226px;
  $s-height: 460px;
  $borderRadius: 3px;
  $prev-position: -84px 50%;
  $prev-position-hover: 0px 50%;
  $next-position: -125px 50%;
  $next-position-hover: -42px 50%;
  @mixin slideSize ($s-width, $s-height) {
    width: $s-width;
    height: $s-height;
  }
  .slide {
    position: relative;
    @include slideSize($s-width, $s-height);
    img {
      @include slideSize($s-width, $s-height);
    }
    .img-box {
      a{
        float: left;
        position: absolute;
      }
      .title {
        position: absolute;
        // padding: 0 15px;
        text-indent: 15px;
        width: $s-width;
        bottom: 0px;
        $height: 52px;
        height: $height;
        line-height: $height;
        background: linear-gradient(to top, rgba(0,0,0,0.6), transparent);
        font: {
          size: 21px;
          family: 'Microsoft YaHei';
          weight: bold;
        }
        color: #fff;
        &:hover {
          color: $blue;
        }
      }
    }
  
    .switch-bar {
      padding: 15px;
      position: absolute;
      right: 0;
      bottom: 5px;
      .switch-btn {
        $rect: 10px;
        width: $rect;
        height: $rect;
        float: left;
        margin-right: 10px;
        border-radius: $rect/2;
        background-color: #f3eae9;
        cursor: pointer;
        transition: 0.3s;
      }
      .cur {
        width: 25px;
        background-color: #ff6041;
      }
    }
    $pn-btn-width: 41px;
    $pn-btn-height: 69px;
    $pn-btn-top: 147px;
    $左按钮left: 234px;
    [name=prev-btn], [name=next-btn] {
      width: $pn-btn-width;
      height: $pn-btn-height;
      position: absolute;
      top: $s-height/2 - $pn-btn-height/2;
      z-index: 2;
      cursor: pointer;
      background-repeat: no-repeat;
    }
    &:hover {
      [name=prev-btn], [name=next-btn]{
        transition: opacity 0.5s;
        opacity: 1;
      }
    }
    [name=prev-btn] {
      left: $左按钮left;
      border-radius: 0 $borderRadius $borderRadius 0;
      background-position: $prev-position;
      &:hover {
        background-position: $prev-position-hover;
      }
    }
    [name=next-btn] {
      right: 0;
      background-position: $next-position;
      border-radius: $borderRadius 0 0 $borderRadius;
      &:hover {
        background-position: $next-position-hover;
      }
    }
  }
  #prev {
    left: -$s-width;
  }
  #next {
    left: $s-width;
  }
.slide ul{
  position: relative;
}
.slide img{
  position: absolute;
  left: 0;
}
.sfade-enter,.sfade-leave-active {
  opacity: 0;
}
.sfade-enter-active, .sfade-leave-active {
  transition: 0.3s;
}
</style>

<template>
  <div class="slide" v-if="slideNews.length" @mouseenter="clearInv" @mouseleave="runInv">
    <ul>
      <li>
        <a :href="slideNews[nowIndex].href" target="_blank">
          <transition name="sfade">
            <img key="one" v-if="isShow" :src="slideNews[nowIndex].src" alt="">
            <img key="two" v-else :src="slideNews[nowIndex].src" alt="">
          </transition>
        </a>
      </li>
    </ul>
    <div class="switch-bar">
      <span class="switch-btn" v-for="(item, index) of slideNews" :class="{cur: index === nowIndex}" @click="goto(index)" :key="index"></span>
    </div>
    <span name="prev-btn" :style="`background-image: url('static/icon-slides.png');`" @click="goto(prevIndex)"></span>
    <span name="next-btn" :style="`background-image: url('static/icon-slides.png');`" @click="goto(nextIndex)"></span>
    <slot>

    </slot>
  </div>
</template>

<script>
import siteData from '../assets/data.js'
export default {
  props: {
    inv: {
      type: Number,
      default: 3.5
    }
  },
  data () {
    return {
      isShow: true,
      slideNews: siteData.headSlide,
      nowIndex: 0,
      showLeft: false,
      showRight: false,
      duration: 0.3, // 左右移动的动画时间 (s)
      nowImgStyle: {
        display: 'block'
      }
    }
  },
  computed: {
    prevIndex () {
      if (this.nowIndex === 0) {
        return this.slideNews.length - 1
      } else {
        return this.nowIndex - 1
      }
    },
    nextIndex () {
      if (this.nowIndex === this.slideNews.length - 1) {
        return 0
      } else {
        return this.nowIndex + 1
      }
    }
  },
  methods: {
    goto (index) {
      this.isShow = !this.isShow
      this.nowIndex = index
    },
    runInv () {
      this.invId = setInterval(() => {
        this.goto(this.nextIndex)
      }, this.inv * 1000)
    },
    clearInv () {
      clearInterval(this.invId)
    }
  },
  mounted () {
    this.runInv()
  }
}
</script>
