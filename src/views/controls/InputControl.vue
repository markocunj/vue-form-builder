<template>
  <div>
    <multiselect
      v-if="control.autocomplete != ''"
      v-model="valueS"
      :options="control.autocomplete"
      :placeholder="control.placeholderText"
      :id="control.uniqueId"
      :name="control.name || control.uniqueId"
      @input="updateValue(valueS)"
    >
    </multiselect>
    <input
      v-else
      :id="control.uniqueId"
      type="text"
      :class="controlFieldClass"
      :value="value"
      :name="control.name || control.uniqueId"
      :placeholder="control.placeholderText"
      @input="updateValue($event.target.value)"
    />
  </div>
</template>

<script>
import { CONTROL_FIELD_EXTEND_MIXIN } from "@/mixins/control-field-extend-mixin";
import Multiselect from "vue-multiselect";
/**
 * InputControl doesn't need to setValue. We can eventually assign the bind the value into the <input>
 * Safe safe...
 */
export default {
  name: "InputControl",
  mixins: [CONTROL_FIELD_EXTEND_MIXIN],
  components: { Multiselect },
  data() {
    return {
      valueS: null,
      test: null,
    };
  },
};
</script>
<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
<style scoped></style>
