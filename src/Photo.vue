<template>
  <figure class="figure" :style="photoStyle" @click="rotate()">
    <img class="photo" :src="src" :alt="title"/>
    <figcaption class="figure-caption">
      <h2>{{title}}</h2>
      <p>{{description}}</p>
    </figcaption>
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
  methods: {
    rotate () {
      console.log(this.isActive)
      if (this.isActive) {
        this.photoStyle.transform = 'rotateY(180deg)'
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

  .photo {
    width: 200px;
    height: 200px;
  }

  .figure-caption {
    margin-top: 20px;
    font-size: 14px;
  }
}
</style>
