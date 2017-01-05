<template>
  <div id="app" class="stage">
    <photo v-for="photo in photos"
      v-on:photoClicked="clickNav($event+1)"
      :idx="photo.idx"
      :src="photo.src"
      :title="photo.title"
      :description="photo.description"
      :is-active="photo.isActive"
      :is-flipped="photo.isFlipped"
      :photo-style="photo.style"
    ></photo>
    <nav>
      <ul class="navs">
        <li class="nav" v-for="n in photos.length"
          :class="{active: n-1==centerPhotoIdx, flipped: n-1==centerPhotoIdx&&centerPhotoFlipped}"
          @click="clickNav(n)"
        ></li>
      </ul>
    </nav>
  </div>
</template>

<script>
import photos from '../data/photos.json';
import Photo from './Photo.vue';

var getRandomIdx = arr => Math.floor(arr.length * Math.random())
var getRandomVal = range => (range[1]-range[0])*Math.random()+range[0]

function getRandomPos(idx, sections) {
  //取模是为了尽量更平均分配到各个区域
  let section = sections[idx % sections.length]
  return {
    x: getRandomVal(section.xRange),
    y: getRandomVal(section.yRange)
  }
}

export default {
  data () {
    return {
      centerPhotoIdx: -1,
      centerPhotoFlipped: false,
      photos: [],
      stage: {
        halfWidth: 0,
        halfHeight: 0
      },
      photo: {
        halfWidth: 160,
        halfHeight: 142
      }
    }
  },
  mounted () {
    this.stage.halfWidth = this.$el.offsetWidth / 2
    this.stage.halfHeight = this.$el.offsetHeight / 2
    this.arrangePhotos(photos)
  },
  components: {
    Photo
  },
  methods: {
    clickNav (n) {
      if (n-1 == this.centerPhotoIdx) {
        this.photos[n-1].isFlipped = !this.photos[n-1].isFlipped
        this.centerPhotoFlipped=!this.centerPhotoFlipped
      } else {
        this.changePhoto(n)
      }
    },
    changePhoto (n) {
      this.centerPhotoIdx = n - 1
      this.arrangePhotos(photos)
    },
    initSections () {
      return [
        { //top left
          xRange: [
            -this.photo.halfWidth,
            this.stage.halfWidth-3*this.photo.halfWidth
          ],
          yRange: [
            -this.photo.halfHeight,
            this.stage.halfHeight-2*this.photo.halfHeight
          ]
        },
        { //top center
          xRange: [
            this.stage.halfWidth-2*this.photo.halfWidth,
            this.stage.halfWidth+2*this.photo.halfWidth,
          ],
          yRange: [
            -this.photo.halfHeight,
            this.stage.halfHeight-4*this.photo.halfHeight
          ]
        },
        { //top right
          xRange: [
            this.stage.halfWidth+this.photo.halfWidth,
            2*this.stage.halfWidth+this.photo.halfWidth
          ],
          yRange: [
            -this.photo.halfHeight,
            this.stage.halfHeight-2*this.photo.halfHeight
          ]
        },
        { //bottom left
          xRange: [
            -this.photo.halfWidth,
            this.stage.halfWidth-3*this.photo.halfWidth
          ],
          yRange: [
            this.stage.halfHeight,
            2*this.stage.halfHeight-2*this.photo.halfHeight
          ]
        },
        { //bottom right
          xRange: [
            this.stage.halfWidth+this.photo.halfWidth,
            2*this.stage.halfWidth-this.photo.halfWidth
          ],
          yRange: [
            this.stage.halfHeight,
            2*this.stage.halfHeight-2*this.photo.halfHeight
          ]
        }
      ]
    },
    arrangePhotos (photos) {
      // 随机找个作为中心图片
      if (this.centerPhotoIdx < 0) {
        this.centerPhotoIdx = getRandomIdx(photos)
      }
      photos.forEach((item, idx) => {
        item.src = './' + item.src
        item.idx = idx
        item.isActive = this.centerPhotoIdx == idx
        item.isFlipped = false
        if (item.isActive) {
          item.style = {
            left: this.stage.halfWidth - this.photo.halfWidth + 'px',
            top: this.stage.halfHeight - this.photo.halfHeight + 'px'
          }
        } else {
          // 随机角度位
          let angle = Math.random() * 90 - 45
          // 在五个区域中随机选择一个，并且在该限定区域内随机一个位置
          let pos = getRandomPos(idx, this.initSections())
          item.style = {
            left: pos.x + 'px',
            top: pos.y + 'px',
            transform: `rotateZ(${angle}deg)`
          }
        }
      })
      this.photos = photos
    }
  }
}
</script>

<style lang="scss">
html {
  height: 100%;
}

body {
  height: 100%;
  margin: 0;
}

h1, h2 {
  font-weight: normal;
  margin: 0;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

.stage {
  position: relative;
  overflow: hidden;
  height: 100%;
  background-color: #eee;

  perspective: 1800px;

  .navs {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 50px;
    text-align: center;
  }

  .nav {
    border-radius: 50%;
    width: 20px;
    height: 20px;
    background-color: #aaa;
    color: #fff;
    text-align: center;
    vertical-align: middle;
    font-size: 12px;
    transition: transform .6s ease-in-out, background-color .6s ease-in-out;

    &:hover {
      cursor: pointer;
    }

    &.active {
      background-color: #888;
      transform: scale(1.5);

      &:after {
        content: "";
        font-family:"iconfont";
        line-height: 20px;
        font-style:normal;
        -webkit-font-smoothing: antialiased;
        -webkit-text-stroke-width: 0.2px;
        -moz-osx-font-smoothing: grayscale;
      }
    }

    &.flipped {
      background-color: #555;
      transform: scale(1.5) rotateY(180deg);
    }
  }
}
</style>
