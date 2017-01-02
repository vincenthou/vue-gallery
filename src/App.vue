<template>
  <div class="stage">
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

function getImages(data) {
  data.forEach((item) => {
    let degrade = Math.random() * 90 - 45
    item.src = './' + item.src
    item.style = {
      left: '20px',
      top: '20px',
      transform: `rotateZ(${degrade}deg)`
    }
  })
  return data
}

export default {
  data () {
    return {
      images: getImages(data)
    }
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
