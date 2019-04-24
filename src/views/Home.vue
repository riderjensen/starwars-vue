<template>
  <div>
    <p v-if="error" style="color: red;">{{error}}</p>
    <v-content>

     <v-layout>

      <v-flex xs12 sm10 offset-sm1>
                           <v-text-field
            v-model="searchName"
            label="Search by name"
            v-on:input="search"
          ></v-text-field>
                                     <v-text-field
            v-model="searchPrice"
            label="Search by price"
            v-on:input="searchByPrice"
            type="number"

          ></v-text-field>
                    <v-btn @click="sortAsc" flat color="blue">Sort Price Ascending</v-btn> 
                    <v-btn @click="sortDsc" flat color="blue">Sort Price Decending</v-btn>          

        <v-card>
          <v-progress-circular v-if="loading" indeterminate color="primary"></v-progress-circular>
          <v-container v-bind="{ [`grid-list-md`]: true }" fluid>
            <v-layout v-if="!loading" row wrap>
              <v-flex v-for="(starship, index) in sortedStarships" :key="index" xs3>
                <v-card min-height="100%">
                  <v-card-title primary-title>
                    <h3 class="headline mb-0">{{starship.name}}</h3>
                  </v-card-title>
                  <v-card-actions>
                    <v-btn class="my-button" @click="selectItem(starship)" :to="starship.url.split('/')[starship.url.split('/').length-2]" flat color="blue">View</v-btn>
                  </v-card-actions>
                </v-card>
              </v-flex>
            </v-layout>
          </v-container>
        </v-card>
      </v-flex>
    </v-layout>
    </v-content>
  </div>
</template>

<script>
import { parse } from 'url';
    export default {
  name: "App",
  created() {
    this.axiosCall("https://swapi.co/api/starships/");
  },
  data() {
    return {
      starships: [],
      sortedStarships: [],
      loading: true,
      selected: {},
      error: '',
      selectedID: 0,
      searchName: '',
      searchPrice: ''
    };
  },
  methods: {
    axiosCall: function(URL) {
      this.axios
        .get(URL)
        .then(response => {
          this.starships.push(...response.data.results);
          this.sortedStarships.push(...response.data.results);
          if (response.data.next) {
            this.axiosCall(response.data.next);
          } else {
            this.loading = false;
          }
        })
        .catch(err => this.error = err);
    },
    selectItem: function(starship){
      this.selected = starship;
      this.selectedID = starship.url.split('/')[starship.url.split('/').length-2]
    },
    sortAsc: function() {
      // FIX ME
      // for(let i = 1; i <= this.starships.length-1; i++){
      //   const tryParse = parseInt(this.starships[i].cost_in_credits);
      //   if(tryParse === NaN){
      //     sortedStarships.push(this.starships[i])
      //   } else {
      //     for(let j = 0; j <= this.sortedStarships.length-1; j++){
      //       if(!(tryParse > parseInt(this.sortedStarships[j].cost_in_credits))){
      //         this.sortedStarships.splice(j, 0, this.starships[i])
            
      //       }
      //     }
      //   }
      // }
      // console.log(this.sortedStarships)
    },
    sortDsc: function() {
    },
    search: function() {
      this.sortedStarships = this.starships.filter(item => item.name.includes(this.searchName))
    },
    searchByPrice: function() {
      this.searchPrice === '' ? this.sortedStarships = [...this.starships] : this.sortedStarships = this.starships.filter(item => parseInt(item.cost_in_credits) < parseInt(this.searchPrice))
    }
  }
};
</script>
<style scoped>
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