<script>
import axios from "axios";
import AppNav from "./assets/components/AppNav.vue";
import AppFooter from "./assets/components/AppFooter.vue";

export default {
  data() {
    return {
      active: true,
      restaurants: "",
    };
  },
  methods: {
    getData() {
      axios.get("http://127.0.0.1:8000/api/data").then((response) => {
        this.arrRest = response.data.restaurants;
        this.resType = response.data.res_type;
        this.arratypes = response.data.types;
        this.arrPlate = response.data.plates;
      });
    },
  },
  created() {
    this.getData();
  },

  components: {
    AppNav,
    AppFooter,
  },
};
</script>

<template>
  <AppNav />
  <router-view></router-view>
  <AppFooter />
</template>

<style lang="scss">
@import "bootstrap/scss/bootstrap";

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
