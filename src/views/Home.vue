<template>
  <div>
    <p v-if="error" style="color: red;">{{error}}</p>
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
                    <v-btn class="my-button" @click="selectItem(starship)" flat color="blue">View</v-btn>
                  </v-card-actions>
                </v-card>
              </v-flex>
            </v-layout>
          </v-container>
        </v-card>
      </v-flex>
    </v-layout>
    <div class="text-xs-center">
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <v-card>
        <v-card-title
          class="headline grey lighten-2"
          primary-title
        >
          {{selected.name}}  
        </v-card-title>

        <v-card-text>
          <p>Model: {{selected.model}}</p>
          <p>Manufacturer: {{selected.manufacturer}}</p>
          <p>Cost: {{selected.cost_in_credits}} credits</p>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            flat
            @click="dialog = false"
          >
            Okay
          </v-btn>
          <!-- send them to individual page -->
                    <v-btn
            color="primary"
            flat
            @click="dialog = false"
          >
            Read More
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
    </v-content>
  </div>
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
      dialog: false,
      error: ''
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
        .catch(err => this.error = err);
    },
    selectItem: function(starship){
      this.selected = starship;
      this.dialog = true;
    }
  }
};
</script>
<style scoped>

</style>