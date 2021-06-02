<template>
  <div :class="[styles.CONTAINER.NORMAL, 'form-padding', 'vue-form-builder']">
    <form>
      <label for="test">Enter the name for your form</label>
      <input
        type="text"
        id="test"
        v-model="name"
        placeholder="Form name"
        class="form-control"
        style="max-width: 300px;"
        required
      /><br />
      <button class="btn btn-info mr-2" @click="saveToLocalStorage()">
        Save your form temporarily. (To test on Test page)
      </button>
    </form>
    <br />
    <br />
    <div class="[styles.CONTAINER.NORMAL, 'form-padding', 'vue-form-builder']">
      <label for="text">Name of the form you want to edit</label>
      <input
        type="text"
        id="text"
        v-model="editName"
        placeholder="Name of the form"
        class="form-control"
        style="max-width: 300px;"
        required
      />
      <br />
      <button class="btn btn-success" @click="getEditData()">
        Edit
      </button>
      <button
        class="btn btn-info"
        style="margin-left: 5px;"
        @click="saveEditedForm(editName)"
      >
        Save edited form
      </button>
      <div v-if="failed">
        <br />
        <div class="alert alert-danger">
          <strong>Error!</strong> Your form doesn't exist
        </div>
      </div>
      <div v-if="nameEmpty">
        <br />
        <div class="alert alert-danger">
          <strong>Error!</strong> You have to enter the name
        </div>
      </div>
    </div>
    <br />
    <br />
    <button class="btn btn-info mr-2" @click="printData()">
      Get JSON Form-Data (Console / Builder)
    </button>
    <br />
    <br />
    <!-- top configuration -->
    <FormConfiguration
      :permissions="permissions"
      v-model="formData.formConfig"
    />

    <!-- form headline -->
    <div
      class="form-headline-container"
      v-show="formData.formConfig.isShowHeadline"
    >
      <h1 v-text="formData.formConfig.headline"></h1>
      <p v-text="formData.formConfig.subHeadline"></p>
    </div>

    <!-- sections of the form -->
    <SectionContainer
      v-for="sectionData in sortedSections"
      :section="sectionData"
      :rows="formData.rows"
      :controls="formData.controls"
      :key="sectionData.uniqueId"
      :permissions="permissions"
    />

    <!-- below all -->
    <AddSectionControl
      v-if="permissions.canAddSection"
      @addSectionNotify="addSection"
    />

    <!-- global stuff -->
    <GlobalSidebar :formData="formData" :permissions="permissions" />
    <GlobalModal :formData="formData" :permissions="permissions" />

    <hr />

    <p class="copyright-text" v-text="copyrightText"></p>
  </div>
</template>

<script>
import AddSectionControl from "@/views/builder/add-controls/AddSectionControl";
import { MAIN_CONSTANTS } from "@/configs";
import SectionContainer from "@/views/builder/SectionContainer";
import FormBuilderBusiness from "@/mixins/form-builder-mixins";
import FormConfiguration from "@/views/builder/FormConfiguration";
import GlobalSidebar from "@/views/builder/GlobalSidebar";
import GlobalModal from "@/views/builder/GlobalModal";
import DefaultPermission from "@/configs/roles";

export default {
  name: "FormBuilder",
  components: {
    GlobalModal,
    GlobalSidebar,
    FormConfiguration,
    SectionContainer,
    AddSectionControl,
  },
  mixins: FormBuilderBusiness,

  props: {
    permissions: {
      type: Object,
      default: () => {
        return DefaultPermission;
      },
    },
  },

  data: () => ({
    formData: {
      formConfig: {},
      sections: {},
      controls: {},
      rows: {},
    },
    name: null,
    editName: null,
    failed: false,
    nameEmpty: false,
  }),

  created() {
    if (this.value && typeof this.value === "object") {
      this.mapping(this.value);
    } else {
      this.createDefaultData();
    }
  },
  methods: {
    printData() {
      console.log(JSON.stringify(this.formData));
    },
    saveToLocalStorage() {
      localStorage.setItem(this.name, JSON.stringify(this.formData));
    },
    getEditData() {
      if (!JSON.parse(localStorage.getItem(this.editName))) {
        this.failed = true;
      } else {
        this.formData = JSON.parse(localStorage.getItem(this.editName));
        this.failed = false;
      }
    },
    saveEditedForm(name) {
      if (!JSON.parse(localStorage.getItem(name))) {
        this.failed = true;
      } else {
        this.failed = false;
        if (name) {
          localStorage.setItem(name, JSON.stringify(this.formData));
          this.nameEmpty = false;
        } else {
          this.nameEmpty = true;
        }
      }
    },
  },
  computed: {
    /**
     * Copyright Text
     * @returns {string}
     */
    copyrightText() {
      return MAIN_CONSTANTS.COPYRIGHT;
    },
  },
};
</script>
<style scoped>
.container {
  max-width: 720px;
}
</style>
