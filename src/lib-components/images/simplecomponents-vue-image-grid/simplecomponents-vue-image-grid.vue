<template>
  <div id="dropzone" class="simple-main">
    <span
      v-if="files != null && files.length != 0"
      :style="
        columns
          ? 'grid-template-columns: repeat(' + columns + ', 1fr) !important'
          : ''
      "
      class="simple-image-grid"
    >
      <div
        v-for="(file, fileIndex) in files"
        :key="fileIndex"
        :style="'background: url(' + file.url + ')'"
        class="simple-background"
      >
        <div class="simple-background-layer">
          <div
            v-if="'icons-' + file.key in $slots || file.text"
            class="simple-background-tag"
          >
            <span v-if="file.text" class="simple-background-label">
              {{ file.text }}
            </span>
            <slot :name="'icons-' + file.key"></slot>
          </div>
        </div>
      </div>
    </span>
    <h4 v-else>
      <span v-if="text">{{ text }}</span>
      <span v-else>No images</span>
    </h4>
    <div v-if="files != null && files.length != 0" class="simple-image-resume">
      <div>
        <span>
          {{ files.length }}
          <span v-if="resume"> {{ resume }}</span>
          <span v-else> images</span>
        </span>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss" src="./simplecomponents-vue-image-grid.scss"></style>

<script>
export default {
  name: 'SimpleComponentsVueImageGrid',
  props: {
    images: [Object, Array],
    text: String,
    resume: String,
    columns: Number,
  },
  data() {
    return {
      files: this.images,
    };
  },
  watch: {
    images() {
      this.files = this.images;
    },
  },
};
</script>
