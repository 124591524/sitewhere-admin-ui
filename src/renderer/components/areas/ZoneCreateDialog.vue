<template>
  <span>
    <zone-dialog :area='area' title="Create Zone" width="600"
      ref="dialog"
      createLabel="Create" cancelLabel="Cancel" mode='create'
      @payload="onCommit">
    </zone-dialog>
  </span>
</template>

<script>
import FloatingActionButton from "../common/FloatingActionButton";
import ZoneDialog from "./ZoneDialog";
import { _createZone } from "../../http/sitewhere-api-wrapper";

export default {
  data: () => ({}),

  components: {
    FloatingActionButton,
    ZoneDialog
  },

  props: ["area"],

  methods: {
    // Send event to open dialog.
    onOpenDialog: function() {
      this.$refs["dialog"].reset();
      this.$refs["dialog"].openDialog();
    },

    // Handle payload commit.
    onCommit: function(payload) {
      var component = this;
      _createZone(this.$store, payload)
        .then(function(response) {
          component.onCommitted(response);
        })
        .catch(function(e) {});
    },

    // Handle successful commit.
    onCommitted: function(result) {
      this.$children[0].closeDialog();
      this.$emit("zoneAdded");
    }
  }
};
</script>

<style scoped>
.add-button {
  position: fixed;
  right: 16px;
  bottom: 16px;
}
</style>
