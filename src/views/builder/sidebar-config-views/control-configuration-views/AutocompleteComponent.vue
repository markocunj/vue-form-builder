<template>
  <SidebarToggleableContainer headline="Autocomplete parts">
    <p style="text-align: justify;">
      &#9432; Choose between adding autocomplete parts by yourself or add a link
      from which the data will come.
    </p>
    <label class="switch">
      <input type="checkbox" v-model="checkbox" />
      <span class="slider round"></span>
    </label>
    <div v-if="!checkbox">
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
    </div>
    <div v-else>
      <div :class="styles.FORM.FORM_GROUP">
        <label>Adding link for parts</label>
        <input type="text" :class="styles.FORM.FORM_CONTROL" v-model="link" />
      </div>
      <div class="buttons">
        <button :class="styles.BUTTON.INFO" @click="gettingAutocompleteParts()">
          Get
        </button>
      </div>

      <br />
      <div class="alert alert-success" v-if="success">
        <strong>Success!</strong> Your files are loaded.
      </div>
      <div class="alert alert-danger" v-if="success == 'false'">
        <strong>Error!</strong> {{ info }}
      </div>
    </div>
    <div>
      <label>Autocomplete default values</label>
      <input
        type="text"
        :class="styles.FORM.FORM_CONTROL"
        v-model="defaultValueParts"
        v-on:keyup.enter="addingToDefault()"
      />
    </div>
    <div class="buttons">
      <button :class="styles.BUTTON.INFO" @click="addingToDefault()">
        Add
      </button>
    </div>
    <div
      :class="styles.FORM.FORM_GROUP"
      class="remove"
      v-for="(parts, tt) in defaultValueArray"
      :key="tt"
    >
      {{ parts }}
      <strong
        ><a
          class="removeButton"
          style="float: right; color: red; cursor: pointer;"
          @click="removingFromDefault(tt)"
          >&#215;</a
        ></strong
      >
    </div>
  </SidebarToggleableContainer>
</template>

<script>
import SidebarToggleableContainer from "@/views/container-views/SidebarToggleableContainer";
import { STYLE_INJECTION_MIXIN } from "@/mixins/style-injection-mixin";
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, axios);

export default {
  name: "AutocompleteComponent",
  mixins: [STYLE_INJECTION_MIXIN],
  data() {
    return {
      defaultValueArray: [],
      defaultValueParts: "",
      arrayWithParts: [],
      autoCompleteParts: "",
      checkbox: null,
      link: "",
      info: null,
      success: null,
    };
  },
  components: { SidebarToggleableContainer },
  props: {
    control: Object,
  },
  methods: {
    gettingAutocompleteParts() {
      Vue.axios
        .get(this.link)
        .then((response) => {
          this.success = true;
          this.control.autocompleteLink = this.link;
        })
        .catch((error) => {
          this.success = false;
          this.info = error.message;
        });
    },
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
    addingToDefault() {
      this.defaultValueArray.push(this.defaultValueParts);
      this.control.defaultValueForAutocomplete = this.defaultValueArray;
      this.defaultValueParts = "";
    },
    removingFromDefault(tt) {
      for (let i = 0; i < this.defaultValueArray.length + 1; ++i) {
        if (tt == this.defaultValueArray.indexOf(this.defaultValueArray[i])) {
          this.defaultValueArray.splice(tt, 1);
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

/* The switch - the box around the slider */
.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

/* Hide default HTML checkbox */
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

/* The slider */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

input:checked + .slider {
  background-color: #2196f3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196f3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
</style>
