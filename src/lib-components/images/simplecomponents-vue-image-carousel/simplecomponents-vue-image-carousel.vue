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
            : ''
        "
        :class="'simple-background-' + componentId"
        class="simple-background"
      >
        <div class="simple-background-layer">
          <div class="simple-background-center">
            <div
              v-if="files != null && files.length != 0 && files.length != 1"
              class="simple-background-arrow"
              @click="previous()"
            >
              <left />
            </div>
            <div v-else></div>
            <div class="simple-background-full">
              <default v-if="files == null || files.length == 0" />
            </div>
            <div
              v-if="files != null && files.length != 0 && files.length != 1"
              class="simple-background-arrow"
              @click="next()"
            >
              <right />
            </div>
            <div v-else></div>
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
import defaultSvg from './components/default.vue';
import rightSvg from './components/right.vue';
import leftSvg from './components/left.vue';

export default {
  name: 'SimpleComponentsVueImageCarousel',
  components: {
    default: defaultSvg,
    right: rightSvg,
    left: leftSvg,
  },
  props: {
    images: [Object, Array],
    auto: Boolean,
    backwards: Boolean,
    interval: Number,
    ratio: String,
  },
  data() {
    return {
      files: this.images,
      pointer: 0,
      previousBackground: '',
      componentId: 'simple-components-vue-image-' + Date.now(),
    };
  },
  created() {
    this.setComponentRatio();
  },
  methods: {
    setComponentRatio() {
      if (this.ratio) {
        const numbers = this.ratio.split(':');
        if (numbers.length == 2) {
          let height = (Number(numbers[1]) / Number(numbers[0])) * 100;
          const css =
            '.simple-background-' +
            this.componentId +
            '::before {padding-bottom: ' +
            height +
            '% !important;}';
          let style = document.getElementById(
            'simple-vue-image-carousel-' + this.componentId
          );
          if (style) {
            style.parentNode.removeChild(style);
          }
          style = document.createElement('style');
          style.id = 'simple-vue-image-carousel-' + this.componentId;
          style.appendChild(document.createTextNode(css));
          document.head.appendChild(style);
        }
      }
    },
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
      if (
        this.auto &&
        this.files &&
        this.files.length != 0 &&
        this.files.length != 1
      ) {
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
    ratio() {
      this.setComponentRatio();
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
