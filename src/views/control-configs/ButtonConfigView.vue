<template>
  <div>
    <div :class="styles.FORM.FORM_GROUP">
      <label>Button Type (Type Attribute)</label>
      <select
        type="text"
        :class="styles.FORM.FORM_CONTROL"
        v-model="control.buttonType"
      >
        <option
          v-for="item in buttonTypeList"
          :key="item"
          :value="item"
          v-text="item"
        >
        </option>
      </select>
    </div>

    <div :class="styles.FORM.FORM_GROUP">
      <label>Button Class (Type Attribute)</label>
      <select
        type="text"
        :class="styles.FORM.FORM_CONTROL"
        v-model="control.buttonClass"
      >
        <option
          v-for="(className, name) in buttonClasses"
          :key="className"
          :value="className"
        >
          {{ name }} ({{ className }})
        </option>
      </select>
    </div>

    <div :class="styles.FORM.FORM_GROUP">
      <label><strong> Link to send your data to: </strong></label>
      <input
        type="text"
        :class="styles.FORM.FORM_CONTROL"
        v-model="control.postLink"
      />
    </div>

    <div :class="styles.FORM.FORM_GROUP">
      <label>
        Run validation before submiting?
        <input type="checkbox" v-model="control.isRunValidation" />
      </label>
    </div>
  </div>
</template>

<script>
import { CONTROL_SPECIAL_CONFIG_MIXIN } from "@/mixins/control-special-config-mixin";

export default {
  name: "ButtonConfigView",
  mixins: [CONTROL_SPECIAL_CONFIG_MIXIN],

  computed: {
    buttonTypeList() {
      return ["button", "submit", "clear"];
    },
    buttonClasses() {
      return this.styles.BUTTON;
    },
  },
};
</script>
