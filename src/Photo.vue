<template>
  <figure class="figure" :class="{flipped:flipped}" :style="{left:photoStyle.left, top:photoStyle.top, transform:photoStyle.transform}" @click="rotate()">
    <img class="photo" :src="src" :alt="title"/>
    <figcaption class="figure-caption">
      <h2>{{title}}</h2>
    </figcaption>
    <p class="description">{{description}}</p>
  </figure>
</template>

<script>
export default {
  props: {
    idx: {
      type: Number,
      required: true
    },
    src: {
      type: String,
      required: true
    },
    title: {
      type: String,
      required: true
    },
    description: {
      type: String,
      required: true
    },
    isActive: {
      type: Boolean,
      required: true
    },
    photoStyle: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      flipped: false
    }
  },
  methods: {
    rotate () {
      if (this.isActive) {
        this.flipped = !this.flipped
      } else {
        this.$emit('photoSelected', this.idx)
      }
    }
  }
}
</script>

<style lang="scss">
.figure {
  position: absolute;
  left: 0;
  top: 0;
  background: #fff;
  padding: 40px;
  margin: 0;
  box-shadow: 2px 2px 8px #ccc;
  transition: ease 1s;

  &:hover {
    cursor: pointer;
  }

  &.flipped {
    transform-style: preserve-3d;
    transform-origin: 0 50% 0;
    transform: translate(280px) rotateY(180deg);
  }

  .photo {
    width: 200px;
    height: 200px;
  }

  .figure-caption {
    margin-top: 20px;
    font-size: 14px;
  }

  .description {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    padding: 40px;
    background: #fff;
    box-sizing: border-box;
    transform: rotateY(180deg) translateZ(1px);
    backface-visibility: hidden;
  }
}
</style>
