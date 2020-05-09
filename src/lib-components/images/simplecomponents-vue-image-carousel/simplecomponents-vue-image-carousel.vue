<template>
  <div
    :style="files != null && files.length != 0 ? previousBackground : ''"
    class="simple-main"
  >
    <transition name="slide-fade" mode="out-in">
      <div
        :key="files != null && files.length != 0 ? files[pointer].url : 'key'"
        :style="
          files != null && files.length != 0
            ? 'background: url(' + files[pointer].url + ')'
            : 'background: url(' + require('./default.svg') + ')'
        "
        class="simple-background"
      >
        <div class="simple-background-layer">
          <div
            v-if="files != null && files.length != 0"
            class="simple-background-center"
          >
            <div class="simple-background-arrow" @click="previous()">
              <img :src="require('./left.svg')" alt="" />
            </div>
            <div class="simple-background-arrow" @click="next()">
              <img :src="require('./right.svg')" alt="" />
            </div>
          </div>
          <div v-if="$slots.default" class="simple-background-tag">
            <slot></slot>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<style
  scoped
  lang="scss"
  src="./simplecomponents-vue-image-carousel.scss"
></style>

<script>
export default {
  name: 'SimpleComponentsVueImageCarousel',
  props: {
    images: [Object, Array],
    auto: Boolean,
    backwards: Boolean,
    interval: Number,
  },
  data() {
    return {
      files: this.images,
      pointer: 0,
      previousBackground: '',
    };
  },
  created() {},
  methods: {
    next() {
      if (this.files) {
        if (this.files.length == this.pointer + 1) {
          this.pointer = 0;
        } else {
          this.pointer++;
        }
      } else {
        this.pointer = 0;
      }
    },
    previous() {
      if (this.files) {
        if (0 == this.pointer) {
          this.pointer = this.files.length - 1;
        } else {
          this.pointer--;
        }
      } else {
        this.pointer = 0;
      }
    },
  },
  watch: {
    images() {
      this.files = this.images;
      if (this.auto && this.files && this.files.length != 0) {
        this.$nextTick(function() {
          window.setInterval(
            () => {
              if (this.backwards) {
                this.previous();
              } else {
                this.next();
              }
            },
            this.interval ? this.interval : 5000
          );
        });
      }
    },
    pointer(newValue, oldValue) {
      if (this.files) {
        this.previousBackground =
          'background: url(' + this.files[oldValue].url + ')';
      } else {
        this.previousBackground = '';
      }
    },
  },
};
</script>
