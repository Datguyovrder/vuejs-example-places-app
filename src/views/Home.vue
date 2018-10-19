<template>
  <div class="home">
    <h1>Interesting Places</h1>

    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>

    <div>
      name: <input v-model="newPlace.name">
      address: <input v-model="newPlace.address">
      <button v-on:click="addPlace()">Add Place</button>
    </div>

    <div>
      <h4>Total Number of Places: {{ places.length }}</h4>
    </div>

    <div v-for="place in places">
      <h3>{{ place.name }}</h3>
      <h3>{{ place.address }}</h3>
      <button v-on:click="deletePlace(place)">Delete Place</button>
    </div>
  </div>
</template>

<style>
</style>

<script>
var axios = require('axios');

export default {
  data: function() {
    return {
      places: [],
      newPlace: {name: "", address: ""},
      errors: []
    };
  },
  created: function() {
    axios
    .get("http://localhost:3000/api/places")
    .then(response => {
      this.places = response.data
    });
  },
  methods: {
    addPlace: function() {
      var params = {
                    name: this.newPlace.name,
                    address: this.newPlace.address
                   };

      axios
      .post("http://localhost:3000/api/places", params)
      .then(response => {
        this.places.push(response.data);
        this.newPlace = {name: "", address: ""}
      })
      .catch(error => {
        console.log(error.response.data.errors);
        this.error = error.response.data.errors;
      });      
    },
    deletePlace: function(inputPlace) {
      var index = this.places.indexOf(inputPlace);
      
      axios
      .delete("http://localhost:3000/api/places/index")
      .then(function(response) {
        this.places.splice(index, 1);
      }.bind(this));
      
    }
  },
  computed: {}
};
</script>