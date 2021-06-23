<template>
  <div class="writers-block">
    <h1>Feeling Lost, Traveler?</h1>
    <div class="card story">
      <div class="card-body">
        <h5 class="card-title">Tropes</h5>
        <p class="card-text">
          {{ trope.name }}
        </p>
      </div>
      <div class="card-body">
        <h5 class="card-title">Archetypes</h5>
        <p class="card-text">
          {{ archetype }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      trope: {},
      archetype: {},
      setting: {},
    };
  },
  mounted: function () {
    this.showTrope();
    this.showArchetype();
  },
  methods: {
    showTrope: function () {
      axios.get("/tropes").then((response) => {
        console.log(response.data);
        this.trope = response.data;
      });
    },
    showSetting: function () {
      axios.get("/settings").then((response) => {
        console.log(response.data);
        this.setting = response.data;
      });
    },
    showArchetype: function () {
      axios.get("/archetypes").then((response) => {
        console.log(response.data);
        this.archetype = response.data;
      });
    },
  },
};
</script>
