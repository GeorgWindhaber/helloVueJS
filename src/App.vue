<template>
  <div id="app">
    <section id="preview">
      <div class="preview-row">
        <label for="#launchLimit">How many launches should be shown?</label>
        <input id="launchLimit" v-model="amountLaunches" type="number" />
        <button id="fetchDataButton" v-on:click="fetchData">Refresh</button>
      </div>
      <input
        class="preview-row"
        v-model="searchText"
        v-on:input="search"
        type="text"
        placeholder="Search..."
      />
      <ul v-if="viewData != null && viewData.length > 0">
        <Preview
          v-for="(launch, index) in viewData"
          :key="index"
          v-bind:name="launch.mission_name"
          v-bind:text="launch.details"
          v-bind:read="launch.read"
          v-on:click.native="launchClick(launch)"
        />
      </ul>
      <p v-else>Sorry, nothing found 😕</p>
    </section>
    <section id="article">
      <Article
        v-if="activeLaunch"
        v-bind:name="activeLaunch.mission_name"
        v-bind:excerpt="activeLaunch.rocket.rocket_name"
        v-bind:text="activeLaunch.details"
      />
    </section>
  </div>
</template>

<script>
import Preview from "./components/Preview.vue";
import Article from "./components/Article.vue";

export default {
  name: "app",
  components: { Preview, Article },
  methods: {
    fetchData: async function() {
      let response = await fetch(
        "https://api.spacexdata.com/v3/launches?limit=" + this.amountLaunches
      );
      let data = await response.json();
      for (let launch of data) {
        launch.read = false;
      }
      this.apiResponse = data;
      this.viewData = data;
    },
    launchClick: function(launch) {
      launch.read = true;
      this.activeLaunch = launch;
    },
    search: function() {
      let cache = [];
      for (let launch of this.apiResponse) {
        if (
          launch.mission_name
            .toLowerCase()
            .includes(this.searchText.toLowerCase())
        ) {
          cache.push(launch);
        }
      }
      this.viewData = cache;
    }
  },
  data: function() {
    return {
      viewData: null,
      apiResponse: null,
      activeLaunch: null,
      amountLaunches: 10,
      searchText: ""
    };
  },
  created() {
    this.fetchData();
  }
};
</script>

<style>
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
}

section {
  width: calc(50% - 1em);
  height: calc(100% - 2em);
  position: fixed;
  margin: 0.5em;
  overflow: scroll;
}

#preview {
  left: 0;
}

#article {
  right: 0;
  border: 1px solid #000;
  border-radius: 5px;
  margin: 0.5em;
}

input {
  border: 1px solid #000;
  border-radius: 5px;
  height: 2em;
}

.preview-row {
  width: calc(100% - 0.5em + 1px);
  margin-bottom: 0.5em;
}

#launchLimit {
  width: 80%;
}

#fetchDataButton {
  padding: 0;
  margin-left: 0.5em;
  height: 2em;
  width: calc(20% - 0.9em);
}
</style>
