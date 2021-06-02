<template>
  <div>
    <div :class="styles.FORM.FORM_GROUP">
      <strong><label>Link to send your files to:</label></strong>
      <input
        type="text"
        :class="styles.FORM.FORM_CONTROL"
        v-model="control.postActionURL"
      />
    </div>

    <div :class="styles.FORM.FORM_GROUP">
      <label>Allowed Extensions</label>
      <input
        type="text"
        :class="styles.FORM.FORM_CONTROL"
        v-model="control.extensions"
      />
    </div>

    <div :class="styles.FORM.FORM_GROUP">
      <label>Default dropzone message</label>
      <input
        type="text"
        :class="styles.FORM.FORM_CONTROL"
        v-model="control.defaultDropzoneMessage"
      />
    </div>

    <div :class="styles.FORM.FORM_GROUP">
      <label>Max Size (in bytes - 0 for unlimited)</label>
      <input
        type="number"
        :class="styles.FORM.FORM_CONTROL"
        v-model="control.maxSize"
      />
    </div>

    <div :class="styles.FORM.FORM_GROUP">
      <label>Multiple?</label> <br />
      <input type="checkbox" v-model="control.isMultiple" />
    </div>

    <div :class="styles.FORM.FORM_GROUP" v-if="control.isMultiple">
      <label>Enter max amount of files:</label>
      <input
        type="number"
        :class="styles.FORM.FORM_CONTROL"
        v-model="control.maximumFiles"
      />
    </div>
  </div>
</template>

<script>
import { CONTROL_SPECIAL_CONFIG_MIXIN } from "@/mixins/control-special-config-mixin";
import GlobalKeyValueItemConfiguration from "@/views/builder/GlobalKeyValueItemConfiguration";

export default {
  name: "FileUploaderConfigView",
  components: { GlobalKeyValueItemConfiguration },
  mixins: [CONTROL_SPECIAL_CONFIG_MIXIN],
};
</script>
