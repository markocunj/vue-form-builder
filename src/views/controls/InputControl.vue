<template>
  <div>
    <multiselect
      v-if="test == true"
      v-model="valueS"
      :value="value"
      :options="control.autocomplete"
      :multiple="true"
      :placeholder="control.placeholderText"
      :id="control.uniqueId"
      :name="control.name || control.uniqueId"
      @input="updateValue(valueS, $event)"
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
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, axios);
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
  mounted() {
    if (this.control.defaultValueForAutocomplete.length > 0) {
      this.valueS = this.control.defaultValueForAutocomplete;
      this.updateValue(this.valueS);
    } else {
      this.valueS = this.control.defaultValue;
      this.updateValue(this.valueS);
    }

    if (
      this.control.autocomplete.length > 0 ||
      this.control.autocompleteLink != ""
    ) {
      this.test = true;
      if (this.control.autocompleteLink != "") {
        Vue.axios
          .get(this.control.autocompleteLink)
          .then((response) => {
            this.control.autocomplete = response.data;
          })
          .catch((error) => {
            console.error(error.message);
          });
      }
    } else {
      this.test = false;
    }
  },
};
</script>
<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
<style scoped></style>
