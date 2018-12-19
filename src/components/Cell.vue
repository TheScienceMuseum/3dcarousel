<template>
  <div class="c-card">
    <div class="c-card__img">
      <img :src="post.image">      
    </div>
    <div class="c-card__title" v-html="post.name"/>
    <p classs="c-card__desc" v-html="post.excerpt"/>
  </div>
</template>

<script>
export default {
  name: "Cell",
  props: {
    post: Object,
    index: Number
  },
  methods: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@mixin type-h($case: uc) {
  line-height: 1;
  font-weight: bold;
  @if $case == uc {
    text-transform: uppercase;
    letter-spacing: 0.015em; // track out if uppercase?
  }
}

@mixin type-meta {
  // nav elements
  font-size: 18px;
  line-height: 1.2;
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 0.015em; // North says track out the headlines?
}

@mixin type-h1($case: uc) {
  // @include between(font-size, rem(40), rem(55));
  @include type-h($case);
}

@mixin type-h2($case: uc) {
  // @include between(font-size, rem(20), rem(30));
  @include type-h($case);
}

.c-card {
  max-width: 100%;
  position: absolute;

  > a {
    display: block;
    height: 100%;
  }

  a {
    text-decoration: none;
    color: currentColor;
    &:hover {
      text-decoration: underline;
    }
  }

  &__img {
    //enforce 4/3 ratio on content
    position: relative;
    overflow: hidden;
    width: 100%;
    height: 0;
    padding-top: 75%;
    background: black;

    img {
      position: absolute;
      top: 0;
      display: block;
      width: 100%;
      height: 100%;
      object-fit: cover;
      font-family: "object-fit: cover;";
      //   @include effect-classes;
    }
  }

  & &__title {
    @include type-h2;
    margin-top: 1rem;
    margin-bottom: 1rem;
  }

  &__info {
    margin-top: 1rem;
    margin-bottom: 1rem;
    @include type-meta;
  }

  &__desc {
    :last-child {
      margin-bottom: 0;
    }
  }

  // @include media(">=small") {
  //   .o-grid--4-col & #{$text-elements} {
  //     // override o-main size on small grids
  //     @include between(font-size, rem(14), rem(18));
  //   }
  // }

  &--panel {
    //dark bg variant
    background-color: black;
    color: white;
  }

  &--panel &__content {
    padding: 0 1em;
  }
}
</style>
