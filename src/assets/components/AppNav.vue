<script>
export default {
  data() {
    return {};
  },
};
</script>

<template>
  <div class="nav">
    <input type="checkbox" id="nav-check" />
    <div class="top-container">
      <div class="nav-header">
        <img class="nav-logo" src="../img/coloratofinale_360.png" alt="" />
      </div>
    </div>
    <div class="nav-btn">
      <label for="nav-check">
        <i class="bi bi-list"></i>
      </label>
    </div>

    <div class="nav-links">
      <a
        href="http://127.0.0.1:8000/"
        id="btn-accedi"
        class="btn-links"
        target="_blank"
      >
        Registrati
      </a>
      <a
        href="http://127.0.0.1:8000/login"
        id="btn-accedi"
        class="btn-links"
        target="_blank"
      >
        Accedi
      </a>
      <router-link
        :to="{ name: 'cart' }"
        class="nav-link active btn-links"
        aria-current="page"
      >
        <i class="bi bi-bag-fill"></i>
      </router-link>
    </div>
  </div>
</template>

<style lang="scss" scoped>
*,
::after,
::before {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.nav {
  height: 90px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #e6e0d7;
  position: relative;
  padding: 1rem;
  z-index: 2000;
}

.nav > .top-container > .nav-header {
  display: inline;
  padding-left: 2rem;
}

.nav > .top-container > .nav-header > .nav-logo {
  max-width: 130px;
  display: inline-block;
}

.nav > .nav-btn {
  display: none;
}

.nav > .nav-links {
  display: inline;
  font-size: 18px;
  padding-right: 2rem;
}

.nav > .nav-links > a {
  display: inline-block;
  font-size: 1.2rem;
  padding: 13px 10px 13px 10px;
  text-decoration: none;
  color: #37363c;
}

.nav > #nav-check {
  display: none;
}

@media (max-width: 600px) {
  .nav > .nav-header > .nav-logo {
    max-width: 120px;
    padding: 10px;
    margin-left: 10px;
  }

  .nav > .nav-btn {
    display: inline-block;
    position: relative;
    position: absolute;
    right: 0px;
    top: 0px;
    padding: 10px;
  }

  .nav > .nav-btn > label {
    display: inline-block;
    width: 50px;
    height: 50px;
    padding: 13px;
  }
  .nav > .nav-btn > label > i {
    display: block;
    font-size: 2rem;
  }

  .nav > .nav-links {
    position: absolute;
    display: block;
    width: 100%;
    background-color: #37363c;
    height: 0px;
    transition: all 0.3s ease-in;
    overflow-y: hidden;
    top: 50px;
    left: 0px;
  }

  .nav > .nav-links > a {
    display: block;
    width: 100%;
    text-align: center;
    color: white;
  }

  .nav > #nav-check:not(:checked) ~ .nav-links {
    height: 0px;
  }

  .nav > #nav-check:checked ~ .nav-links {
    //height: calc(100vh - 150px);
    height: 200px;
    margin-top: 2.5rem;
    overflow-y: auto;
    background-color: rgba(0, 0, 0, 0.5);
  }
}
</style>
