<template>
  <v-dialog v-model="dialog" max-width="400px">
    <template v-slot:activator="{ on }">
      <v-btn block v-on="on" class="mt-0 mb-0">+</v-btn>
    </template>
    <AddJobDialog :stage="stage" :url="url" @clickedSth="closeDialog()" />
  </v-dialog>
</template>

<script>
import AddJobDialog from "../components/AddJobDialog.vue";
export default {
  props: ["stage", "url"],
  components: {
    AddJobDialog
  },

  data() {
    return {
      dialog: false
    };
  },
  methods: {
    closeDialog: function() {
      this.dialog = false;
      this.$emit("updateJobEvent");
    }
  }
};
</script>