<template>
  <SidebarToggleableContainer headline="Autocomplete parts">
    <div :class="styles.FORM.FORM_GROUP">
      <label>Adding autocomplete parts</label>
      <input
        type="text"
        :class="styles.FORM.FORM_CONTROL"
        v-model="autoCompleteParts"
        v-on:keyup.enter="addingToArray()"
      />
    </div>
    <div class="buttons">
      <button :class="styles.BUTTON.INFO" @click="addingToArray()">
        Add
      </button>
    </div>
    <div
      :class="styles.FORM.FORM_GROUP"
      class="remove"
      v-for="(part, index) in arrayWithParts"
      :key="index"
    >
      {{ part }}
      <strong
        ><a
          class="removeButton"
          style="float: right; color: red; cursor: pointer;"
          @click="removingFromArray(index)"
          >&#215;</a
        ></strong
      >
    </div>
  </SidebarToggleableContainer>
</template>

<script>
import SidebarToggleableContainer from "@/views/container-views/SidebarToggleableContainer";
import { STYLE_INJECTION_MIXIN } from "@/mixins/style-injection-mixin";

export default {
  name: "AutocompleteComponent",
  mixins: [STYLE_INJECTION_MIXIN],
  data() {
    return {
      arrayWithParts: [],
      autoCompleteParts: "",
    };
  },
  components: { SidebarToggleableContainer },
  props: {
    control: Object,
  },
  methods: {
    addingToArray() {
      this.arrayWithParts.push(this.autoCompleteParts);
      this.control.autocomplete = this.arrayWithParts;
      this.autoCompleteParts = "";
    },
    removingFromArray(index) {
      for (let i = 0; i < this.arrayWithParts.length + 1; ++i) {
        if (index == this.arrayWithParts.indexOf(this.arrayWithParts[i])) {
          this.arrayWithParts.splice(index, 1);
        }
      }
    },
  },
};
</script>

<style>
.remove {
  margin-top: 10px;
  padding: 10px;
  border: 1px solid;
  border-width: thin;
  border-radius: 5px;
  border-color: rgb(173, 173, 173);
  color: black;
  cursor: pointer;
}
.remove:hover {
  background-color: rgb(197, 194, 194);
}
.removeButton:hover {
  color: red;
}
</style>
