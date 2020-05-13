<template>
  <div
    id="dropzone"
    @click="uploadImages()"
    @dragover.prevent
    @drop.prevent
    @drop="handleFileDrop"
    class="simple-main"
  >
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
          <div class="simple-background-tag">
            {{ file.document.name.substring(0, 15) }}
          </div>
        </div>
      </div>
    </span>
    <h4 v-else>
      <span v-if="text">{{ text }}</span>
      <span v-else><strong>Press here </strong>or drag the images</span>
    </h4>
    <div v-if="files != null && files.length != 0" class="simple-image-resume">
      <div>
        <span v-if="error" class="simple-image-resume-error">
          <small>[{{ error.timeout }}]</small>
          {{ maximumText ? maximumText : error.message }}
        </span>
        <span>
          {{ files.length }}
          <span v-if="resume"> {{ resume }}</span>
          <span v-else> images</span>
        </span>
      </div>
    </div>
    <input
      type="file"
      @change="imageFunction"
      ref="inputFile"
      class="simple-hidden"
      :name="inputName + '[]'"
      multiple="multiple"
      :id="inputName"
      accept="image/*"
    />
  </div>
</template>

<style scoped lang="scss" src="./simplecomponents-vue-form-images.scss"></style>

<script>
export default {
  name: 'SimpleComponentsVueFormImages',
  props: {
    container: [Object, Array],
    text: String,
    name: String,
    columns: Number,
    resume: String,
    maximum: Number,
    maximumText: String,
    timeout: Number,
  },
  data() {
    return {
      files: [],
      error: null,
      inputName:
        this.name != null || this.name != undefined
          ? this.name
          : 'input-simple-name-' + Date.now(),
    };
  },
  methods: {
    uploadImages() {
      if (this.error == null) {
        document.getElementById(this.inputName).click();
      }
    },
    handleFileDrop(e) {
      if (this.error == null) {
        let droppedFiles = e.dataTransfer.files;
        if (!droppedFiles) return;
        this.saveFiles(droppedFiles);
      }
    },
    imageFunction() {
      let inputFiles = this.$refs.inputFile.files;
      this.saveFiles(inputFiles);
    },
    saveFiles(files) {
      this.files = [];
      for (let inputFile of files) {
        let reader = new FileReader();
        let tempFile = {
          url: '',
          document: '',
        };
        reader.addEventListener(
          'load',
          function() {
            tempFile.url = reader.result;
          }.bind(this),
          false
        );
        if (inputFile) {
          if (/\.(png|jpg|jpeg|PNG|JPG|JPEG)$/i.test(inputFile.name)) {
            reader.readAsDataURL(inputFile);
            tempFile.document = inputFile;
            this.files.push(tempFile);
          } else {
            console.log('the image format is not supported');
          }
        }
      }
    },
  },
  watch: {
    files() {
      if (this.maximum) {
        if (this.files && this.files.length > this.maximum) {
          if (this.timeout) {
            var timeout = this.timeout;
          } else {
            var timeout = 2000;
          }
          var startTime = Date.now();
          this.error = {
            message: 'Maximum files exceeded',
            type: 'maximum',
            timeout: Math.floor(timeout / 1000),
          };
          let interval = setInterval(() => {
            var now = Date.now();
            this.error.timeout = Math.floor(
              (timeout - (now - startTime)) / 1000
            );
            if (timeout - (now - startTime) <= 200) {
              clearInterval(interval);
              return;
            }
          }, 1);
          setTimeout(() => {
            this.files = [];
          }, timeout + 200);
        } else {
          this.error = null;
          this.$emit('files', this.files);
        }
      } else {
        this.error = null;
        this.$emit('files', this.files);
      }
    },
  },
};
</script>
