<template>
  <v-app>
    <v-content>
      <v-container style="padding: 0; padding-bottom: 20px" fluid fill-height>
        <v-layout justify-center>
          <v-flex text-xs-center>
            <router-view/>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>  
</template>

<script>
export default {
  name: "App",
  created() {
    this.axiosCall("https://swapi.co/api/starships/");
  },
  data() {
    return {
      starships: [],
      loading: true,
      selected: {},
      dialog: false
    };
  },
  methods: {
    axiosCall: function(URL) {
      this.axios
        .get(URL)
        .then(response => {
          this.starships.push(...response.data.results);
          if (response.data.next) {
            this.axiosCall(response.data.next);
          } else {
            console.log(this.starships)
            this.loading = false;
          }
        })
        .catch(err => console.log(err));
    },
    selectItem: function(starship){
      this.selected = starship;
      this.dialog = true;
    }
  }
};
</script>

<style>
.v-card__actions {
  position: absolute;
  bottom: 0;
  width: 100%;
}
.my-button {
  width: 100%;
}
.v-card__title--primary {
  margin-bottom: 40px;
}
</style>
