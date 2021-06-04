<template>
  <vue-dropzone
    :id="control.uniqueId"
    ref="Dropzone"
    :options="dropzoneOptions"
  ></vue-dropzone>
</template>

<script>
import { CONTROL_FIELD_EXTEND_MIXIN } from "@/mixins/control-field-extend-mixin";
import vue2Dropzone from "vue2-dropzone";
import "vue2-dropzone/dist/vue2Dropzone.min.css";
export default {
  components: {
    vueDropzone: vue2Dropzone,
  },
  mixins: [CONTROL_FIELD_EXTEND_MIXIN],

  data() {
    return {
      dropzoneOptions: {},
    };
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
    };
  },
};
</script>
