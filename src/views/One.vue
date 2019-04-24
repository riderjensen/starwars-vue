<template>
    <div>
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
    <p>Pilots: {{ship.pilots.length >= 1 ? ship.pilots : "None"}}</p>
    </div>
</template>

<script>
export default {
    data(){
        return{
            ship:{}
        }
    },
    created(){
        this.axios.get(
        `https://swapi.co/api/starships/${
          this._routerRoot._route.params.id
        }/`,
      )
      .then((resp) => {
        console.log(resp.data);
        this.ship = resp.data;
      })
      .catch((err) => {
        this.error = err;
      });
    }
}
</script>
