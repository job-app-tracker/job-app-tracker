<template>
  <div>
    <v-dialog v-model="dialog" max-width="900" persistent="true">
      <template v-slot:activator="{ on }">
        <v-hover>
          <v-card
            v-on="deleteHover ? '' : on"
            slot-scope="{ hover }"
            xs12
            dark
            :color="job.color"
            :max-width="width"
            height="70"
            class="mt-2 pb-4"
          >
            <v-list-tile :class="job.color" class="pt-2">
              <v-list-tile-avatar :size="24" class="pb-2 pl-2">
                <v-img
                  :src="`//logo.clearbit.com/${job.image}.com`"
                  alt="avatar"
                ></v-img>
              </v-list-tile-avatar>

              <v-list-tile-content>
                <v-list-tile-title class="font-weight-regular">
                  {{ job.company }}
                </v-list-tile-title>
                <v-list-tile-sub-title class="font-weight-light">
                  {{ job.title }}
                </v-list-tile-sub-title>
              </v-list-tile-content>

              <v-list-tile-action v-if="hover">
                <v-btn
                  icon
                  ripple
                  v-on:click="deleteJob(job)"
                  @mouseover="mouseOver"
                  @mouseout="mouseOut"
                >
                  <v-icon color="grey lighten-1"> delete_outline </v-icon>
                </v-btn>
              </v-list-tile-action>
            </v-list-tile>
            <v-list-tile-content
              class="text-xs-right pr-3 pt-4"
              :class="job.color"
            >
              <v-list-tile-sub-title class="font-weight-light caption">
                added {{ job.date_added }}
              </v-list-tile-sub-title>
            </v-list-tile-content>
          </v-card>
        </v-hover>
      </template>
      <JobDialog :job="job" @clickedClose="closeDialog()" />
    </v-dialog>
  </div>
</template>

<script>
import JobDialog from "./JobDialog.vue";

export default {
  name: "JobCard",
  props: ["job", "url"],
  components: { JobDialog },
  data: function() {
    return {
      deleteHover: false,
      dialog: false,
      width: 400
      // url: "https://fast-citadel-67812.herokuapp.com"
    };
  },
  methods: {
    closeDialog: function() {
      this.dialog = false;
      this.$emit("updateInfoEvent", this.job);
    },
    mouseOver: function() {
      this.deleteHover = true;
      console.log(this.deleteHover);
    },
    mouseOut: function() {
      this.deleteHover = false;
      console.log(this.deleteHover);
    },
    deleteJob: function(job) {
      fetch(`${this.url}/jobs/${job._id}`, {
        method: "DELETE"
      }).then(response => {
        if (response.status == 204) {
          console.log("It worked");
          this.$emit("removeJobEvent");
        } else if (response.status == 400) {
          response.json().then(data => {
            alert(data.msg);
          });
        }
      });
    }
  }
};
</script>

<style></style>
