<template>
  <figure class="figure" :class="{flipped:isFlipped}" :style="{left:photoStyle.left, top:photoStyle.top, transform:photoStyle.transform}" @click="clickPhoto()">
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
    isFlipped: {
      type: Boolean,
      required: true
    },
    photoStyle: {
      type: Object,
      required: true
    }
  },
  methods: {
    clickPhoto () {
      this.$emit('photoClicked', this.idx)
    }
  }
}
</script>

<style lang="scss">
.figure {
  position: absolute;
  left: 0;
  top: 0;
  background-color: #fff;
  padding: 40px;
  margin: 0;
  box-shadow: 2px 2px 8px #ccc;
  transform-style: preserve-3d;
  transform-origin: 0 50% 0;
  transition: transform .6s ease-in-out, left .6s ease-in-out, top .6s ease-in-out;

  &:hover {
    cursor: pointer;
  }

  &.flipped {
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
    background-color: #fff;
    box-sizing: border-box;
    transform: rotateY(180deg) translateZ(1px);
    backface-visibility: hidden;
  }
}
</style>
