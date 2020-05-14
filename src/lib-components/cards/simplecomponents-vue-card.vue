<template>
  <div class="simple-card-main">
    <div class="simple-card-image">
      <simple-carousel :images="images">
        <div v-if="badgeText" :slot="'content-0'" class="simple-card-badge">
          <span
            :style="
              badgeColor
                ? 'background-color: ' + badgeColor + ';'
                : 'background-color: rgba(0,0,0,0.7);'
            "
          >
            {{ badgeText }}
          </span>
        </div>
      </simple-carousel>
    </div>
    <div v-if="$slots.default" class="simple-card-separator" />
    <div>
      <slot></slot>
    </div>
  </div>
</template>

<style scoped lang="scss" src="./simplecomponents-vue-card.scss"></style>

<script>
import SimpleComponentsVueImageCarousel from '@/lib-components/images/simplecomponents-vue-image-carousel/simplecomponents-vue-image-carousel.vue';

export default {
  name: 'SimpleComponentsVueCard',
  components: {
    'simple-carousel': SimpleComponentsVueImageCarousel,
  },
  props: {
    image: String,
    contentPadding: Boolean,
    badgeText: String,
    badgeColor: String,
  },
  data() {
    return {
      images: this.image
        ? [
            {
              key: 0,
              url: this.image,
            },
          ]
        : [],
    };
  },
  methods: {},
  watch: {
    image() {
      this.images = this.image
        ? [
            {
              key: 0,
              url: this.image,
            },
          ]
        : [];
    },
  },
};
</script>
