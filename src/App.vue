<template>
  <v-app>
    <v-content>
     <v-layout>
      <v-flex xs12 sm10 offset-sm1>
        <v-card>
          <v-progress-circular v-if="loading" indeterminate color="primary"></v-progress-circular>
          <v-container v-bind="{ [`grid-list-md`]: true }" fluid>
            <v-layout v-if="!loading" row wrap>
              <v-flex v-for="(starship, index) in starships" :key="index" xs3>
                <v-card min-height="100%">
                  <v-card-title primary-title>
                    <h3 class="headline mb-0">{{starship.name}}</h3>
                  </v-card-title>
                  <v-card-actions>
                    <v-btn class="my-button" flat color="blue">View</v-btn>
                  </v-card-actions>
                </v-card>
              </v-flex>
            </v-layout>
          </v-container>
        </v-card>
      </v-flex>
    </v-layout>
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
      loading: true
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
