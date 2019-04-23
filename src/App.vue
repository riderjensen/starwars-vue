<template>
  <v-app>
    <v-content>
      <v-progress-circular v-if="loading" indeterminate color="primary"></v-progress-circular>
      <v-container v-bind="{ [`grid-list-md`]: true }" fluid>
        <v-layout v-if="!loading" row wrap>
          <v-flex v-for="(starship, index) in starships" :key="index" xs3>
            <ShipCard />
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import ShipCard from "./components/ShipCard";

export default {
  name: "App",
  components: {
    ShipCard
  },
  created() {
    this.axiosCall("https://swapi.co/api/starships/");
  },
  data() {
    return {
      starships: [],
      loading: true
    };
  },
  methods: {
    axiosCall: function(URL) {
      this.axios
        .get(URL)
        .then(response => {
          console.log(response.data);
          this.starships.push(...response.data.results);
          if (response.data.next) {
            this.axiosCall(response.data.next);
          } else {
            this.loading = false;
          }
        })
        .catch(err => console.log(err));
    }
  }
};
</script>
