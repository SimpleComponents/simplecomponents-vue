<script>
import Vue from 'vue';
import {
  SimplecomponentsVueFormImages,
  SimplecomponentsVueImageGrid,
  SimplecomponentsVueImageCarousel,
} from '@/entry';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    SimplecomponentsVueFormImages,
    SimplecomponentsVueImageGrid,
    SimplecomponentsVueImageCarousel,
  },
  data() {
    return {
      files: [],
      images: [],
    };
  },
  methods: {
    printAlert(element) {
      alert(element);
    },
  },
  watch: {
    files() {
      let i = 0;
      this.images = this.files.map(file => {
        file.key = i;
        file.text = 'text';
        i++;
        return file;
      });
    },
  },
});
</script>

<template>
  <div class="simple-container" id="app">
    <div class="simple-container-son">
      <SimplecomponentsVueFormImages
        name="images-testing"
        text="Presiona aquí o arrastra las imagenes"
        :columns="4"
        resume="Imagenes"
        @files="
          data => {
            this.files = data;
          }
        "
      />
      <br />
      <SimplecomponentsVueImageGrid
        :images="images"
        text="Sin Imagenes"
        resume="Imagenes"
        :columns="4"
      >
        <div
          v-for="(image, imageIndex) in images"
          :key="imageIndex"
          :slot="'icons-' + image.key"
        >
          <i @click="printAlert(image.key)">
            link
          </i>
          <i @click="printAlert(image.key)">
            delete
          </i>
        </div>
      </SimplecomponentsVueImageGrid>
      <br />
      <SimplecomponentsVueImageCarousel :images="images" :auto="true">
      </SimplecomponentsVueImageCarousel>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.simple-container {
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.simple-container-son {
  width: 50%;
}
</style>
