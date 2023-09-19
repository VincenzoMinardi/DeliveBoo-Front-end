<script>
import axios from "axios";
export default {
  data() {
    return {
      restShow: [],
      listPlate: [],
      alertResId: false,
      alertPlate: false,
    };
  },
  methods: {
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
    setListPlate() {
      if (localStorage.getItem("cart") !== null) {
        if (JSON.parse(localStorage.getItem("cart"))[0].res_id == this.$route.params.restaurant) {
          this.alertResId = false
          this.listPlate = JSON.parse(localStorage.getItem("cart"))
        } else {
          this.alertResId = true
          this.restShow.forEach(element => {
            this.listPlate.push({ id: element.id, quantit: 0, res_id: element.restaurant_id })
          })
        }
      } else {
        this.restShow.forEach(element => {
          this.listPlate.push({ id: element.id, quantit: 0, res_id: element.restaurant_id })
        })
      }
    },
    getRestaurantImageUrl(img) {
      return `/img/${img}`;
    },
    destroyStorage() {
      localStorage.clear()
      console.log("Sono Morte, il distruttore di mondi");
    },
    modPlate(id, sign, key) {
      if (localStorage.getItem("cart") !== null) {
        if (JSON.parse(localStorage.getItem("cart"))[0].res_id == this.$route.params.restaurant) {
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
        } else {
          this.alertPlate = true
        }
      } else {
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

    }
  },
  computed: {
    uniqueRestaurants() {
      // Raggruppa i piatti per ristorante
      const restaurantMap = new Map();
      this.restShow.forEach((item) => {
        const { restaurant_id, rest_name, address, img } = item;
        if (!restaurantMap.has(restaurant_id)) {
          restaurantMap.set(restaurant_id, {
            id: restaurant_id,
            rest_name,
            address,
            img,
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
  <div class="rest-container" v-for="restaurant in uniqueRestaurants" :key="restaurant.id">
    <div class="restName">
      <div class="restLogo">
        <img :src="getRestaurantImageUrl(restaurant.img)" alt="" />
      </div>
      <div class="title">
        <h1>{{ restaurant.rest_name }}</h1>
        <p>{{ restaurant.address }}</p>
      </div>
    </div>

    <div class="alert alert-danger my-3" v-if="alertResId">
      Attenzione, sei su un altro ristorante, non potrai ordinare piatti da qui.
    </div>
    <div class="alert alert-warning" role="alert" v-if="alertPlate">
      Attenzione, non puoi ordinare da un altro ristorante.
    </div>

    <div class="dish-container">
      <div class="card mb-3 plates" v-for="(plate, key) in restaurant.plates" :key="plate.id">
        <div class="row g-0 dish-card">
          <div class="col-md-8">
            <div class="card-body">
              <h5 class="card-title cardText">{{ plate.name }}</h5>
              <p class="card-text cardText ingredients">
                {{ plate.ingredients }}
              </p>
              <p class="card-text cardText">
                € {{ Math.round(plate.price / 100).toFixed(2) }}
              </p>
            </div>
          </div>

          <div class="col-md-4 add">
            <div class="d-flex justify-content-center align-items-center switch-dock">
              <div class="d-flex justify-content-center align-items-center joycon-left"
                @click="modPlate(plate.id, `-`, key)">
                -
              </div>
              <div class="d-flex justify-content-center align-items-center switch-screen">
                {{ listPlate[key].quantit }}
              </div>
              <div class="d-flex justify-content-center align-items-center joycon-right"
                @click="modPlate(plate.id, `+`, key)">
                +
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <router-link :to="{
      name: 'home',
    }" class="arrow-back">
      <i class="bi bi-arrow-left-circle-fill btn-back"></i>
    </router-link>
  </div>
  <button @click="destroyStorage()" style="background-color: red; width: 100px; height: 50px; color: aliceblue">
    Nuke
  </button>
</template>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Permanent+Marker&display=swap");

.rest-container {
  background-color: #e6e0d7;
  padding: 1rem 0.9rem;
  cursor: pointer;

  .restName {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-radius: 3rem;
    padding: 2rem;
    background-color: rgb(231, 165, 80);

    .restLogo {
      flex: 0 0 40%;

      img {
        width: 100%;
        border-radius: 500rem;
      }
    }

    .title {
      flex: 0 0 60%;
      text-align: center;

      h1 {
        font-family: "Permanent Marker", cursive;
        font-size: 3.5rem;
      }

      p {
        font-size: 2rem;
        font-style: italic;
      }
    }
  }

  .dish-container {
    max-width: 1200px;
    padding: 0.8rem;
    margin: 1rem auto;
    display: flex;
    flex-wrap: wrap;
    align-items: stretch;
    justify-content: flex-start;
    padding: 0.5rem;
  }

  .plates {
    flex: 0 0 45%;
    //border: 1px solid #37363c;
    border-radius: 2.5rem;
    margin-right: 2rem;
    padding: 1rem;
    box-shadow: 0 0 4px rgba(0, 0, 0, 0.4);
    background-color: #e6e0d7;
  }

  .dish-card {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;

    .cardText {
      flex: 0 0 60%;
    }

    .ingredients {
      font-size: 0.8rem;
    }

    .add {
      flex: 1 0 0;
      display: flex;
      justify-content: center;
    }
  }

  .arrow-back {
    font-size: 2rem;
    padding: 1rem 2rem;

    i {
      color: #37363c;
    }
  }
}

// bottone per aggiungere o togliere piatti
.switch-dock {
  stroke-width: 1px;
  border: 1px solid #37363d;
  width: 92px;
  height: 52px;
  border-radius: 5px;
}

.joycon-left {
  width: 20px;
  height: 40px;
  border-radius: 5px 0px 0px 5px;
  background: #f9f7ed;
  animation-fill-mode: forwards;
  animation-play-state: paused;

  &:active {
    animation-play-state: running;
  }
}

@keyframes color-change-left {
  0% {
    background-color: #f9f7ed;
  }

  50% {
    background-color: #00c3e3;
  }

  100% {
    background-color: #f9f7ed;
  }
}

.joycon-right {
  width: 20px;
  height: 40px;
  border-radius: 0px 5px 5px 0px;
  background: #f9f7ed;
  animation: color-change-right 0.1s;
  animation-play-state: paused;

  &:active {
    animation-play-state: running;
  }
}

@keyframes color-change-right {
  0% {
    background-color: #f9f7ed;
  }

  50% {
    background-color: #ff4554;
  }

  100% {
    background-color: #f9f7ed;
  }
}

.switch-screen {
  width: 40px;
}

//MEDIA QUERY PER MOBILE SCREEN 320/500px

@media (max-width: 500px) {

  //container generale
  .rest-container {
    width: 100%;

    //container del ristoratore
    .restName {
      width: 100%;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      flex-wrap: wrap;
      padding: 0.3rem;

      .restLogo {
        flex: 0 0 100%;

        img {
          max-width: 250px;
          text-align: center;
          display: block;
          margin-bottom: 0.5rem;
          margin-top: 0.5rem;
        }
      }

      .title {
        flex: 0 0 100%;
        text-align: center;
        line-height: 0.2rem;
        padding: 0.4rem;
      }

      h1 {
        font-size: 2.3rem;
      }

      p {
        font-size: 0.8rem;
      }
    }

    //container piatti

    .dish-container {
      width: 100%;
      flex-direction: column;
    }

    //card piatti

    .plates {
      flex: 0 0 100%;
      margin-right: 0;
      border: 0;
    }

    //contenuto di ogni card
    .dish-card {
      display: flex;
      flex-direction: column;
      align-items: flex-start;

      .card-body {
        flex: 0 0 100%;
      }

      .ingredients {
        font-size: 0.8rem;
      }
    }
  }
}
</style>
