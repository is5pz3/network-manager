<template>
  <div>
    <v-dialog v-model="dialog" max-width="600px">
      <template v-slot:activator="{ on }">
        <v-btn @click="dialog=true" color="primary" flat>
          Add
          <v-icon right>add_circle_outline</v-icon>
        </v-btn>
      </template>

      <v-card color="white" hide-overlay>
        <v-toolbar dark color="primary">
          <v-toolbar-title>Add New Composite Measure</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn icon dark @click="dialog = false">
            <v-icon>close</v-icon>
          </v-btn>
        </v-toolbar>

        <v-layout column class="pa-4">
          <v-flex>
            <v-select v-model="selectedSensor" :items="getAllSensors" label="Select Sensor"></v-select>
          </v-flex>

          <v-flex>
            <v-subheader class="pl-0">Last</v-subheader>
            <v-slider v-model="lastMinute" thumb-label="always"></v-slider>
          </v-flex>

          <v-flex>
            <v-subheader class="pl-0">Every</v-subheader>
            <v-slider v-model="everyMinute" thumb-label="always"></v-slider>
          </v-flex>
        </v-layout>
        <v-alert
          :value="errorMessageCreateComposite"
          color="error"
          icon="warning"
          outline
        >{{errorMessageCreateComposite}}</v-alert>
        <v-alert :value="addMessage" type="success" outline>Composite measure was created</v-alert>
        <v-divider></v-divider>
        <v-btn @click="addNewCompositeMeasure" flat color="primary">Save</v-btn>
        <v-btn @click="dialog = false" flat>Cancel</v-btn>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
import { mapState } from "vuex";

export default {
  data() {
    return {
      dialog: false,
      lastMinute: 5,
      everyMinute: 1,
      selectedSensor: ""
    };
  },
  computed: {
    ...mapGetters({
      getAllSensors: "getAllSensors",
      getToken: "getToken"
    }),
    ...mapState(["errorMessageCreateComposite", "addMessage"])
  },

  methods: {
    addNewCompositeMeasure() {
      var payload = {
        sensor_id: this.selectedSensor,
        time_window: this.lastMinute,
        calculation_frequency: this.everyMinute,
        token: this.getToken
      };
      this.$store.dispatch("saveNewCompositeMeasure", payload);
      // this.dialog = false;
    }
  }
};
</script>

<style>
</style>
