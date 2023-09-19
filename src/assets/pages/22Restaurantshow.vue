<script>
import axios from "axios";
export default {
  data() {
    return {
      restShow: [],
      listPlate: [],
    };
  },
  methods: {
    setListPlate() {
      if (localStorage.getItem("cart") !== null) {
        this.listPlate = JSON.parse(localStorage.getItem("cart"))
      } else {
        this.restShow.forEach(element => {
          this.listPlate.push({ id: element.id, quantit: 0 })
        })
      }
    },
    getPaltes() {
      axios
        .get(
          "http://127.0.0.1:8000/api/restaurants/" +
          String(this.$route.params.restaurant)
        )
        .then(response => (
          this.restShow = response.data,
          this.setListPlate()
        ));
    },
    destroyStorage() {
      localStorage.clear()
      console.log("Sono Morte, il distruttore di mondi");
    },
    modPlate(id, sign, key) {
      document.querySelectorAll(".text-danger").forEach(element => {
        element.innerHTML = ""
      })

      if (sign == "-") {
        let quantit = this.listPlate[key].quantit
        quantit--
        if (quantit <= -1) {
          this.listPlate[key].quantit = 0
        } else {
          this.listPlate[key].quantit--
        }
      } else {
        this.listPlate[key].quantit++
      }

      localStorage.setItem('cart', JSON.stringify(this.listPlate))

    }
  },
  computed: {
    uniqueRestaurants() {
      // Raggruppa i piatti per ristorante
      const restaurantMap = new Map();
      this.restShow.forEach((item) => {
        const { restaurant_id, rest_name, address } = item;
        if (!restaurantMap.has(restaurant_id)) {
          restaurantMap.set(restaurant_id, {
            id: restaurant_id,
            rest_name,
            address,
            plates: [],
          });
        }
        restaurantMap.get(restaurant_id).plates.push(item);
      });
      return Array.from(restaurantMap.values());
    },
  },

  created() {
    this.getPaltes()
  },
};
</script>

<template>
  <!-- <div
    class="rest-container"
    v-for="restaurant in uniqueRestaurants"
    :key="restaurant.id"
  >
    <div>
      <div class="restaurant">
        <img :src="restShow.img" :alt="restShow.rest_name" />
        <h1>{{ restaurant.rest_name }}</h1>
        <p>{{ restaurant.address }}</p>
      </div>
      <div class="plates" v-for="plate in restaurant.plates" :key="plate.id">
        <h2>{{ plate.name }}</h2>
        <div>ingredienti: {{ plate.ingredients }}</div>
        <div>{{ plate.price }}€</div>
      </div>
    </div>
  </div> -->
  <div class="rest-container" v-for="restaurant in uniqueRestaurants" :key="restaurant.id">
    <div class="restName">
      <h1>{{ restaurant.rest_name }}</h1>
      <p>{{ restaurant.address }}</p>
    </div>

    <div class="dish-container">
      <div class="card plates" style="width: 18rem" v-for="plate, key in restaurant.plates" :key="plate.id">
        <ul class="list-group list-group-flush">
          <li class="list-group-item">{{ plate.name }}</li>
          <li class="list-group-item">ingredienti: {{ plate.ingredients }}</li>
          <li class="list-group-item">
            € {{ Math.round(plate.price / 100).toFixed(2) }}
          </li>
          <li class="list-group-item">
            <input type="number" :name="plate.id" :id="plate.id" value="'modplate' + 'plate.id'">
          </li>
          <li class="list-group-item" style="background-color: burlywood;">
            <div style="width:30px; height: 30px; background-color: black; margin: 5px" @click="addPlate(plate.id)"></div>
            <div class="d-flex justify-content-center align-items-center"
              style="stroke-width: 1px; border:1px solid #37363D; width: 92px; height: 52px; border-radius: 5px;">
              <div class="d-flex justify-content-center align-items-center"
                style="width: 20px; height: 40px; border-radius: 5px 0px 0px 5px; background: #F9F7ED;"
                @click="modPlate(plate.id, `-`, key)">
                -
              </div>
              <div class="d-flex justify-content-center align-items-center" style="width: 40px;">
                {{ listPlate[key].quantit }}
              </div>
              <div class="d-flex justify-content-center align-items-center"
                style="width: 20px; height: 40px; border-radius: 0px 5px 5px 0px; background: #F9F7ED;"
                @click="modPlate(plate.id, `+`, key)">
                +
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
  <button @click="destroyStorage()"
    style="background-color: red; width: 100px; height: 50px; color: aliceblue;">Nuke</button>
</template>

<style lang="scss" scoped>
.dish-container {
  display: flex;
  flex-wrap: wrap;
  max-width: 1100px;
  align-items: center;
  justify-content: flex-start;
  padding-inline: 3rem;
  margin: 1rem auto;
}
</style>
