<script>
import Vue from 'vue';
import { SimplecomponentsVueFormImages } from '@/entry';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    SimplecomponentsVueFormImages,
  }
});
</script>

<template>
  <div class="simple-container" id="app">
    <div class="simple-container-son">
      <SimplecomponentsVueFormImages
        name="images-testing"
        text="Presiona aquí o arrastra las imagenes"
        @files="
          data => {
            files = data;
          }
        "
      />
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
