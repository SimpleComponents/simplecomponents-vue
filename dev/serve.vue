<script>
import Vue from 'vue';
import {
  SimplecomponentsVueFormImages,
  SimplecomponentsVueImageGrid,
  SimplecomponentsVueImageCarousel,
  SimplecomponentsVueCard,
} from '@/entry';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    SimplecomponentsVueFormImages,
    SimplecomponentsVueImageGrid,
    SimplecomponentsVueImageCarousel,
    SimplecomponentsVueCard,
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
    images() {
      //console.log(this.images);
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
      <br />
      <div class="simple-list-cards">
        <SimplecomponentsVueCard
          :contentPadding="true"
          :image="images[0] ? images[0].url : ''"
        >
          <div class="simple-list-card-content">
            <strong>category</strong>
            <h3>Titulo</h3>
            <p>
              Lorem ipsum dolor sit amet consectetur, adipisicing elit. Nihil
              debitis provident dolorem quod voluptatibus eveniet quos incidunt
            </p>
            <span>
              $ 1990
            </span>
            <br />
            <small>
              ***** (34 views)
            </small>
          </div>
        </SimplecomponentsVueCard>
        <SimplecomponentsVueCard
          :badgeText="'category'"
          :image="images[0] ? images[0].url : ''"
        >
          <div class="simple-list-card-content">
            <strong>category</strong>
            <h3>Titulo</h3>
            <p>
              Lorem ipsum dolor sit amet consectetur, adipisicing elit. Nihil
              debitis provident dolorem quod voluptatibus eveniet quos incidunt
            </p>
            <span>
              $ 1990
            </span>
            <br />
            <small>
              ***** (34 views)
            </small>
          </div>
        </SimplecomponentsVueCard>
        <SimplecomponentsVueCard :image="images[0] ? images[0].url : ''">
        </SimplecomponentsVueCard>
        <SimplecomponentsVueCard :image="images[0] ? images[0].url : ''">
        </SimplecomponentsVueCard>
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

.simple-list-cards {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
  width: 100%;
  > div {
    margin-right: 10px;
  }
}

.simple-list-card-content {
  > strong {
    font-size: 11px;
    color: rgba($color: #000000, $alpha: 0.65);
    text-transform: uppercase;
    margin: 0px;
  }
  > h3 {
    margin: 0px;
    margin-bottom: 2px;
    font-size: 18px;
    color: rgba($color: #000000, $alpha: 0.9);
  }
  > p {
    font-size: 14px;
    color: rgba($color: #000000, $alpha: 0.6);
    margin: 0px;
    margin-bottom: 5px;
    padding: 0px;
  }
  > span {
    font-size: 16px;
  }
  > small {
    font-size: 14px;
    color: rgba($color: #000000, $alpha: 0.6);
    margin: 0px;
    margin-bottom: 5px;
    padding: 0px;
  }
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
