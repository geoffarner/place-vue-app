<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "That Vue-do You Do So Well",
      places: [],
      newPlaceParams: {},
      currentPlace: {},
      editPlaceParams: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("/places.json").then((response) => {
        this.places = response.data;
        console.log("All Places:", this.places);
      });
    },
    createPlace: function () {
      axios
        .post("/places.json", this.newPlacesParams)
        .then((response) => {
          console.log("New Places.", response.data);
          this.places.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showPlace: function (place) {
      console.log(place);
      this.currentPlace = place;
      document.quereySelector("/#place-details").showModal();
    },
    updatePlaces: function (place) {
      axios.patch("/places/" + place.id + ".json", this.editPlaceParams).then((response) => {
        console.log("Place Updated", response.data);
      });
    },
    destroyPlace: function (place) {
      axios.delete("/places/" + place.id).then((response) => {
        console.log("Place Deleted.", response.data);
        var index = this.places.indexOf(place);
        this.places.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      Name:
      <input type="text" v-model="newPlaceParams.name" />
      Address:
      <input type="text" v-model="newPlaceParams.address" />
      <button v-on:click="createPlace()">Make a New Place</button>
    </div>
    <div v-for="place in places" v-bind:key="place.id">
      <p>{{ place.name }}</p>
      <p>{{ place.address }}</p>
      <button v-on:click="showPlace(place)">Show Place</button>
    </div>
  </div>
</template>

<style></style>
