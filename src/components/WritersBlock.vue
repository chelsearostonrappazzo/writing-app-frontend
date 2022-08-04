<template>
  <div class="writers-block">
    <h3 class="writers-heading">Feeling Lost, Traveler?</h3>
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Tropes</h5>
        <p class="card-text">
          {{ trope }}
        </p>
        <button class="btn btn-primary outline writers" v-on:click="showTrope()">generate</button>
        <button class="btn btn-primary outline writers" v-on:click="clearTrope()">clear</button>
      </div>
    </div>
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Archetypes</h5>
        <p class="card-text">
          {{ archetypes }}
        </p>
        <input type="text" v-model="archetypeNumber" placeholder="how many?" />
        <button class="btn btn-primary outline writers" v-on:click="showArchetype(archetypeNumber)">generate</button>
        <button class="btn btn-primary outline writers" v-on:click="clearArchetypes()">clear</button>
      </div>
    </div>
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Settings</h5>
        <p class="card-text">
          {{ setting }}
        </p>
        <button class="btn btn-primary outline writers" v-on:click="showSetting()">generate</button>
        <button class="btn btn-primary outline writers" v-on:click="clearSetting()">clear</button>
      </div>
    </div>
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">Prompt</h5>
        <p class="card-text">
          {{ prompt }}
        </p>
        <input type="text" v-model="archetypeNumber" placeholder="how many characters?" />
        <button class="btn btn-primary outline writers" v-on:click="showPrompt(promptNumber)">generate</button>
        <button class="btn btn-primary outline writers" v-on:click="clearPrompt()">clear</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      trope: "",
      archetypes: "",
      setting: "",
      prompt: "",
      archetypeNumber: "",
      promptNumber: "",
    };
  },
  mounted: function () {},
  methods: {
    showTrope: function () {
      axios.get("/trope").then((response) => {
        this.trope = response.data;
      });
    },
    showSetting: function () {
      axios.get("/setting").then((response) => {
        this.setting = response.data;
      });
    },
    showArchetype: function (archetypeNumber) {
      axios.get(`/archetypes?number=${archetypeNumber}`).then((response) => {
        this.archetypes = response.data;
      });
    },
    showPrompt: function (promptNumber) {
      axios.get(`/prompt?number=${promptNumber}`).then((response) => {
        this.prompt = response.data;
      });
    },
    clearArchetypes: function () {
      this.archetypeNumber = "";
      this.archetypes = "";
    },
    clearSetting: function () {
      this.setting = "";
    },
    clearTrope: function () {
      this.trope = "";
    },
    clearPrompt: function () {
      this.promptNumber = "";
      this.prompt = "";
    },
  },
};
</script>
