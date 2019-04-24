<template>
    <div>
    <p v-if="error" style="color: red;">{{error}}</p>

        <v-progress-circular v-if="loading" indeterminate color="primary"></v-progress-circular>
       <div v-if="!loading">
           <p>Name: {{ship.name}}</p>
    <p>Model: {{ship.model}}</p>
    <p>Manufacturer{{ship.manufacturer}}</p>
    <p>Cost: {{ship.cost_in_credits}}</p>
    <p>Length: {{ship.length}}</p>
    <p>Atmosphering Speed: {{ship.max_atmosphering_speed}}</p>
    <p>Crew: {{ship.crew}}</p>
    <p>Passengers: {{ship.passengers}}</p>
    <p>Cargo Capacity: {{ship.cargo_capacity}}</p>
    <p>Consumables: {{ship.consumables}}</p>
    <p>Class: {{ship.starship_class}}</p>
    <v-btn v-if="ship.pilots.length >= 1" class="my-button" @click="pilots(0)" flat color="blue">View Pilots</v-btn>
             
       </div>
        <v-progress-circular v-if="pilotLoading" indeterminate color="primary"></v-progress-circular>
       <div v-if="!pilotLoading && returnedPilots.length >1">
          <div v-for="(incPilot, index) in returnedPilots" :key="index">
                <p>Pilot Name: {{incPilot.name}}</p>
             <p>Birth: {{incPilot.birth_year}}</p>
             <p>Gender: {{incPilot.gender}}</p>
             <p>Eye Color: {{incPilot.eye_color}}</p>
             <p>Hair Color: {{incPilot.hair_color}}</p>
          </div>
           
       </div>
    
    </div>
</template>

<script>
export default {
    data(){
        return{
            ship:{},
            error: '',
            loading: true,
            returnedPilots: [],
            pilotLoading: false,
        }
    },
    created(){
        this.axios.get(
        `https://swapi.co/api/starships/${
          this._routerRoot._route.params.id
        }/`,
      )
      .then((resp) => {
        this.loading = false;
        this.ship = resp.data;
      })
      .catch((err) => {
          this.loading = false
        this.error = err;
      });
    },
    methods:{
        pilots: function(number){
        this.pilotLoading = true;
        this.axios.get(this.ship.pilots[number])
        .then(response => {
          this.returnedPilots.push(response.data);
          number < this.ship.pilots.length-1 ? this.pilots(number + 1) : this.pilotLoading = false;
        })
        .catch(err => this.error = err);
        }
    }
}
</script>
