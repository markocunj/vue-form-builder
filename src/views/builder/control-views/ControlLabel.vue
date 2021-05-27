<template>
  <div class="control-label-container">
    <label
      v-text="control.label"
      :for="control.uniqueId"
      :class="control.additionalLabelClass"
    >
    </label>
    <i v-if="isRequired" style="color: red" v-text="' *'"></i>
    <br v-if="control.subLabel" />
    <small v-if="control.subLabel" v-text="control.subLabel"> </small>
  </div>
</template>

<script>
export default {
  name: "ControlLabel",
  props: {
    control: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      isRequired: false,
    };
  },
  mounted() {
    for (let i = 0; i < this.control.validations.length; i++) {
      if (this.control.validations[i].ruleType == "required") {
        this.isRequired = true;
      }
    }
  },
};
</script>

<style scoped></style>
