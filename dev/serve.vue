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
        text="Presiona aquí o arrastra las imagenes (5 Max.)"
        :columns="4"
        :maximum="5"
        maximumText="límite excedido"
        :timeout="4000"
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
      <div class="simple-column">
        <div class="simple-column-first">
          <SimplecomponentsVueImageCarousel
            :images="images"
            :auto="true"
            :ratio="'2:1'"
          >
            <div
              v-for="(image, imageIndex) in images"
              :key="imageIndex"
              :slot="'content-' + image.key"
              @click="printAlert('image with key: ' + image.key)"
              class="simple-test-container-no-shadow"
            >
              <div>
                <h3>Image with key {{ image.key }}</h3>
                <p>
                  Lorem ipsum dolor sit amet consectetur adipisicing elit.
                  Nulla, magnam voluptates natus maiores doloremque ea pariatur
                  id. Reprehenderit atque blanditiis veritatis eos iste aut
                  laboriosam aspernatur dicta dolores, doloremque deserunt.
                </p>
              </div>
            </div>
          </SimplecomponentsVueImageCarousel>
        </div>
        <div class="simple-column-second">
          <SimplecomponentsVueImageCarousel :images="images">
            <div
              v-for="(image, imageIndex) in images"
              :key="imageIndex"
              :slot="'content-shadow-' + image.key"
              @click="printAlert('image with key: ' + image.key)"
              class="simple-test-container"
            >
              <div>
                <h3>Image with key {{ image.key }}</h3>
                <p>
                  Lorem ipsum dolor sit amet consectetur adipisicing elit.
                  Nulla, magnam voluptates natus maiores doloremque ea pariatur
                  id. Reprehenderit atque blanditiis veritatis eos iste aut
                  laboriosam aspernatur dicta dolores, doloremque deserunt.
                </p>
              </div>
            </div>
          </SimplecomponentsVueImageCarousel>
        </div>
      </div>
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
  padding-top: 40em;
  padding-bottom: 40em;
}

.simple-test-container {
  height: 100%;
  padding-left: 30px;
  padding-top: 30px;
  padding-right: 30px;
  padding-bottom: 20px;
}

.simple-test-container-no-shadow {
  height: 100%;
  padding-left: 30px;
  padding-top: 30px;
  padding-right: 30px;
}

.simple-column {
  width: 100%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;

  .simple-column-first {
    width: 66.7%;
    padding-right: 5px;
  }
  .simple-column-second {
    width: 33.4%;
    padding-left: 5px;
  }
}

.simple-container-son {
  width: 90%;
}
</style>
