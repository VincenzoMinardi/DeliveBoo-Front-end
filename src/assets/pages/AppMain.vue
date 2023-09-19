<script>
import axios from "axios";
import AppType from "../components/AppType.vue";
import AppJumbo from "../components/AppJumbo.vue";
import AppRestaurantCard from "../components/AppRestaurantCard.vue";
export default {
  data() {
    return {
      linkImg: "",
      randomPerTe: "",
      randomResoult: "",
      arrTypes: {},
      arrRest: {},
      resType: {},
      arrPlate: {},
      selectedRest: [],
      tests: [],
      forza: [],
      filteredRest: [],
      isActive: 0,
    };
  },
  components: {
    AppType,
    AppJumbo,
    AppRestaurantCard,
  },
  methods: {
    rndNumber() {
      this.randomPerTe =
        Math.floor(Math.random() * Object.keys(this.arrTypes).length) + 1;
      this.randomResoult = this.arrTypes[this.randomPerTe].description;
    },
    addImg(img) {
      return `assets/img/${img}`;
    },
    getData() {
      axios.get("http://127.0.0.1:8000/api/data").then((response) => {
        this.arrRest = response.data.restaurants;
        this.resType = response.data.res_type;
        this.arrTypes = response.data.types;
        this.arrPlate = response.data.plates;

        this.printRest();
        this.rndNumber();
      });
    },

    pushID(id) {
      this.tests = [];
      this.isActive = true,
      this.tests.push(id);
      this.forza = [];
      this.selectedRest = [];
      this.filteredRest = [];    
      this.resType.forEach((daje) => {
        if (daje.type_id == id) {
          this.forza.push(daje.restaurant_id)
        }
      });
      this.printRest();
    },
    printRest() {
      this.forza.forEach((su) => {
        this.selectedRest = this.arrRest.find(selectedRest => selectedRest.id === su);
        this.filteredRest.push(this.selectedRest);
    });
    }
  },
  created() {
    this.getData();
  },
};
</script>
<template>
  <AppJumbo />
  <div>
    <div class="container-fluid">
      <div class="container-type">
        <h1 class="title">I tuoi piatti preferiti, consegnati da noi</h1>
        <div class="container d-flex justify-content-center flex-wrap gap-4">
          <AppType v-for="(singleType, index) in arrTypes" :id="singleType.id" :singleType="singleType" :isActive="isActiveArray"
            @click="pushID(singleType.id)" />
        </div>
      </div>
      <div class="cont-text" style="background: linear-gradient(267deg, #9f672e 2.83%, #37363d 97.17%)">
        <h2 class="d-inline-block text-light">Per te:</h2>
        <span class="text-light fs-5">
          {{ this.randomResoult }}
        </span>
      </div>
      <div class="container d-flex align-items-center mt-5">
        <div class="row">
          <div class="col-xxl-3 col-xl-4 col-lg-4 col-md-4 col-sm-6 col-12 d-flex align-items-center justify-content-start mb-4" v-for="  restaurant   in   filteredRest  " :key="restaurant.id">
            <AppRestaurantCard :restaurant="restaurant"/>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Permanent+Marker&display=swap");
.container-type {
  border-radius: 3rem;
  padding: 1rem;
  width: 100%;
  background-color: rgb(231, 165, 80);
}

.title {
  font-size: 3rem;
  color: rgb(55, 54, 60);
  padding: 1.5rem 2rem;
  text-align: center;
  font-family: "Permanent Marker", cursive;
}

.cont-text {
  padding: 1rem;
  margin-top: 1rem;
  //background-color: rgb(55, 54, 60);
  border-radius: 2rem;
  font-style: italic;

  h2 {
    font-size: 1.3rem;
  }

  span {
    font-size: 1rem;
  }
}

@media (max-width: 600px) {
  .title {
    font-size: 2.5rem;
  }
}
</style>
