<template>
  <figure class="figure" :style="image.style">
    <img class="image" :src="image.src" :alt="image.title"/>
    <figcaption class="figure-caption">
      <h2>{{image.title}}</h2>
    </figcaption>
  </figure>
</template>

<script>
export default {
  props: {
    image: {
      type: Object,
      required: true
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
  padding: 20px;
  margin: 0;

  .image {
    width: 200px;
    height: 200px;
  }

  .figure-caption {
    margin-top: 20px;
    font-size: 14px;
  }
}
</style>
