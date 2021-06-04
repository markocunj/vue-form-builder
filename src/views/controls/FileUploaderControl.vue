<template>
  <vue-dropzone
    :id="control.uniqueId"
    ref="Dropzone"
    :options="dropzoneOptions"
    @vdropzone-success="afterComplete"
    @vdropzone-removed-file="removedFile"
  ></vue-dropzone>
</template>

<script>
import { CONTROL_FIELD_EXTEND_MIXIN } from "@/mixins/control-field-extend-mixin";
import vue2Dropzone from "vue2-dropzone";
import "vue2-dropzone/dist/vue2Dropzone.min.css";
export default {
  name: "FileUploaderComponent",
  components: {
    vueDropzone: vue2Dropzone,
  },
  mixins: [CONTROL_FIELD_EXTEND_MIXIN],

  data() {
    return {
      dropzoneOptions: {},
      files: null,
      array: [],
      test: null,
    };
  },
  methods: {
    afterComplete(file) {
      this.array.push(file.name);
      this.updateValue(this.array);
    },
    removedFile(value) {
      for (let i = 0; i < this.array.length; ++i) {
        if (value.name == this.array[i]) {
          let index = this.array.indexOf(this.array[i]);
          this.array.splice(index, 1);
          if (this.array.length < 1) {
            this.setValue(this.test);
          } else this.updateValue(this.array);
        }
      }
    },
  },
  created() {
    this.dropzoneOptions = {
      url: this.control.postActionURL,
      maxFileSize: this.control.maxSize,
      uploadMultiple: this.control.isMultiple,
      acceptedFiles: this.control.extensions,
      addRemoveLinks: true,
      maxFiles: this.control.maximumFiles,
      dictDefaultMessage: this.control.defaultDropzoneMessage,
      dictInvalidFileType: "Invalid file.",
      dictFileTooBig: "File is too big.",
      dictMaxFilesExceeded: "Max files added",
    };
  },
};
</script>
