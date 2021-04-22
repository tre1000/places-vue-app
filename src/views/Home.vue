<template>
  <div>
    {{ places }}
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      places: [],
      placeName: "",
      placeAddress: "",
      currentPlace: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios
        .get("/api/places")
        .then((response) => {
          this.places = response.data;
        })
        .catch((error) => console.log(error.response));
    },
    createPlace: function () {
      var params = {
        name: this.placeName,
        address: this.placeAddress,
      };
      axios
        .post("/api/places", params)
        .then((response) => {
          console.log("Place added!");
          this.places.push(response);
        })
        .catch((error) => console.log(error.response));
    },
    showPlace: function (place) {
      this.currentPlace = place;
      document.querySelector("#product-details").showModal();
    },
    updatePlace: function (place) {
      var params = {
        name: place.name,
        address: place.address,
      };
      axios.patch("api/places/" + place.id, params).then((response) => {
        console.log("Update successful", response.data);
      });
    },
  },
};
</script>
