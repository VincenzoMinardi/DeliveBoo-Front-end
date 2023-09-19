<script>
export default {
  props: {
    restaurant: Array,
  },

  data() {
    return {
      img: "/img/" + this.restaurant.img,
    };
  },
};
</script>

<template>
  <router-link :to="{
    name: 'restaurants.show',
    params: { restaurant: restaurant.id },
  }" class="card h-100">
    <img class="card-img-top img" :src="img" :alt="restaurant.name" />
    <div class="card-body">
      <h5 class="card-title">{{ restaurant.rest_name }}</h5>
      <p class="card-text">{{ restaurant.address }}</p>
    </div>
  </router-link>
</template>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Permanent+Marker&display=swap");

.card {
  min-width: 200px;
  text-decoration: none;
  color: rgb(55, 54, 60);
  //border: 1px solid rgb(55, 54, 60);
  box-shadow: 0 0 6px rgba(0, 0, 0, 0.6);
  border-radius: 2rem;
}

.card img {
  border-top-left-radius: 2rem;
  border-top-right-radius: 2rem;
  box-shadow: 0 0 4px rgb(55, 54, 60);
}

.card-title {
  font-family: "Permanent Marker", cursive;
  font-size: 2rem;
}

.card-text {
  font-style: italic;
  font-size: 1.2rem;
}
</style>
