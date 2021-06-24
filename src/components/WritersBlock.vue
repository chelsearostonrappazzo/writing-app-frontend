<template>
  <div class="writers-block">
    <h3>Feeling Lost, Traveler?</h3>
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Tropes</h5>
        <p class="card-text" v-for="trope in trope" :key="trope.id">
          {{ trope.name }}
        </p>
        <button class="btn btn-primary b outline" v-on:click="showTrope()">generate</button>
      </div>
    </div>
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Archetypes</h5>
        <p class="card-text" v-for="archetype in archetypes" :key="archetype.id">
          {{ archetype.name }}
        </p>
        <input type="text" v-model="archetypeNumber" placeholder="how many?" />
        <button class="btn btn-primary outline" v-on:click="showArchetype(archetypeNumber)">generate</button>
        <button class="btn btn-primary outline" v-on:click="clearArchetypes()">clear</button>
      </div>
    </div>
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Settings</h5>
        <p class="card-text" v-for="setting in settings" :key="setting.id">
          {{ setting.name }}
        </p>
        <input type="text" v-model="settingNumber" placeholder="how many?" />
        <button class="btn btn-primary outline" v-on:click="showSetting(settingNumber)">generate</button>
        <button class="btn btn-primary outline" v-on:click="clearSettings()">clear</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      trope: [],
      archetypes: [],
      settings: [],
      archetypeNumber: "",
      settingNumber: "",
    };
  },
  mounted: function () {},
  methods: {
    showTrope: function () {
      axios.get("/trope").then((response) => {
        console.log(response.data);
        this.trope = response.data;
      });
    },
    showSetting: function (settingNumber) {
      axios.get(`/setting?number=${settingNumber}`).then((response) => {
        console.log(response.data);
        this.settings = response.data;
      });
    },
    showArchetype: function (archetypeNumber) {
      axios.get(`/archetype?number=${archetypeNumber}`).then((response) => {
        console.log(response.data);
        this.archetypes = response.data;
      });
    },
    clearArchetypes: function () {
      this.archetypeNumber = "";
      this.archetypes = [];
    },
    clearSettings: function () {
      this.settingNumber = "";
      this.settings = [];
    },
  },
};
</script>
