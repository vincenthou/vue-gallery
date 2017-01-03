<template>
  <div id="app" class="stage">
    <photo class="figure" v-for="image in images" :image="image"></photo>
    <nav>
      <ul class="navs">
        <li class="nav" v-for="n in images.length">{{n}}</li>
      </ul>
    </nav>
  </div>
</template>

<script>
import data from '../data/images.json';
import Photo from './Photo.vue';

var getRandomIdx = arr => Math.floor(arr.length * Math.random())
var getRandomVal = range => (range[1]-range[0])*Math.random()+range[0]

function getRandomPos(idx, sections) {
  let section = sections[idx % sections.length]
  console.log(section)
  return {
    x: getRandomVal(section.xRange),
    y: getRandomVal(section.yRange)
  }
}

function initSections(sizes) {
  return [
    { //top left
      xRange: [
        -sizes.photo.halfWidth,
        sizes.stage.halfWidth-3*sizes.photo.halfWidth
      ],
      yRange: [
        -sizes.photo.halfHeight,
        sizes.stage.halfHeight-2*sizes.photo.halfHeight
      ]
    },
    { //top center
      xRange: [
        sizes.stage.halfWidth-2*sizes.photo.halfWidth,
        sizes.stage.halfWidth+2*sizes.photo.halfWidth,
      ],
      yRange: [
        -sizes.photo.halfHeight,
        sizes.stage.halfHeight-3*sizes.photo.halfHeight
      ]
    },
    { //top right
      xRange: [
        sizes.stage.halfWidth+sizes.photo.halfWidth,
        2*sizes.stage.halfWidth+sizes.photo.halfWidth
      ],
      yRange: [
        -sizes.photo.halfHeight,
        sizes.stage.halfHeight-2*sizes.photo.halfHeight
      ]
    },
    { //bottom left
      xRange: [
        -sizes.photo.halfWidth,
        sizes.stage.halfWidth-3*sizes.photo.halfWidth
      ],
      yRange: [
        sizes.stage.halfHeight,
        2*sizes.stage.halfHeight-2*sizes.photo.halfHeight
      ]
    },
    { //bottom right
      xRange: [
        sizes.stage.halfWidth+sizes.photo.halfWidth,
        2*sizes.stage.halfWidth-sizes.photo.halfWidth
      ],
      yRange: [
        sizes.stage.halfHeight,
        2*sizes.stage.halfHeight-2*sizes.photo.halfHeight
      ]
    }
  ]
}

function getImages(data, sizes) {
  // 随机找个作为中心图片
  let centerIdx = getRandomIdx(data)
  data.forEach((item, idx) => {
    item.src = './' + item.src
    if (centerIdx == idx) {
      item.style = {
        left: sizes.stage.halfWidth - sizes.photo.halfWidth + 'px',
        top: sizes.stage.halfHeight - sizes.photo.halfHeight + 'px'
      }
    } else {
      // 随机角度位
      let angle = Math.random() * 360
      // 在五个区域中随机选择一个，并且在该限定区域内随机一个位置
      let pos = getRandomPos(idx, initSections(sizes))
      item.style = {
        left: pos.x + 'px',
        top: pos.y + 'px',
        transform: `rotateZ(${angle}deg)`
      }
    }
  })
  return data
}

export default {
  data () {
    return {
      images: []
    }
  },
  mounted () {
    let sizes = {
      stage: {
        halfWidth: 0,
        halfHeight: 0
      },
      photo: {
        halfWidth: 140,
        halfHeight: 142
      }
    }
    sizes.stage.halfWidth = this.$el.offsetWidth / 2
    sizes.stage.halfHeight = this.$el.offsetHeight / 2
    this.images = getImages(data, sizes)
    console.log(sizes)
  },
  components: {
    Photo
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
  background: #eee;

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
    line-height: 20px;
    background: #222;
    color: #fff;
    text-align: center;
    font-size: 12px;
  }
}
</style>
