<template>
  <div id="main_page" container="container-fluid">
    <!-- header -->
    <div class="header p-3 d-flex justify-content-center align-items-center">
      <h3 class="text-center m-0">Names</h3>
    </div>
    <div>
      <!-- button -->
      <div class="d-flex justify-content-end align-items-center p-3">
        <button class="btn btn-primary" v-on:click="reorganize">
          Reorganize
        </button>
      </div>
      <!-- page loader -->
      <div
        class="d-flex justify-content-center align-items-center loader"
        v-if="loading"
      >
        <div
          class="spinner-grow d-flex justify-content-center align-items-center"
          role="status"
        >
          <span class="sr-only"></span>
        </div>
      </div>
      <!-- error handler -->
      <div class="text-center error p-3" v-if="error">
        <p>An error ocurred. Please check internet connection and try again.</p>
      </div>
      <!-- cards of names -->
      <div class="card-container p-3">
        <div
          class="
            card
            custom-card
            border
            d-flex
            justify-content-center
            align-items-center
          "
          v-for="card in cards"
          :key="card.id"
        >
          <p class="text-center">{{ card.name }}</p>
        </div>
        <!-- <div class="end mt-4" v-if="length">
          <p class="text-center">------  END OF NAMES -----</p>
        </div> -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MainPage",
  data() {
    return {
      cards: [],
      // length: this.data().cards.length,
      loading: true,
      error: false,
    };
  },
  mounted() {
    fetch("https://random-data-api.com/api/name/random_name?size=12")
      .then((res) => res.json())
      .then((data) => {
        this.loading = false;
        this.error = false;
        this.cards = data;
      })
      .catch((err) => {
        this.error = true;
        this.loading = false;
        console.log(err);
      });
  },
  methods: {
    reorganize: function () {
      this.cards = [];
      this.loading = true;
      this.error = false;
      fetch("https://random-data-api.com/api/name/random_name?size=12")
        .then((res) => res.json())
        .then((data) => {
          this.loading = false;
          this.error = false;
          this.cards = data;
        })
        .catch((err) => {
          this.loading = false;
          this.error = true;
          console.log(err);
        });
    },
  },
};
</script>

<style>
body {
  background: #f8f8f8;
  overflow-x: hidden;
  font-family: 'Roboto', sans-serif;
}
* {
  transition: all 0.2s ease-in-out;
  box-sizing: border-box;
}
.header {
  width: 100vw;
  background: #0d6efd;
  color: #fff;
}
.loader {
  margin-top: 100px;
  width: 100vw;
}
.btn {
  cursor: pointer;
  /* width: 100px;
  height: 50px; */
}
.card-container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: repeat(auto-fill, minmax(250px,1fr));
}
.card {
  background: #fff;
  font-size: 1.5rem;
  font-weight: normal;
  word-wrap: break-word;
  white-space: nowrap;
  height: 250px;
  cursor: pointer;
}
.card:hover {
  -webkit-box-shadow: 0 10px 10px #777;
  -moz-box-shadow: 0 10px 10px #777;
  box-shadow: 0 10px 10px #777;
  -webkit-transform: rotate(-2deg);
  -moz-transform: rotate(-2deg);
  -o-transform: rotate(-2deg);
  -ms-transform: rotate(-2deg);
  transform: rotate(-2deg);
}
.error {
  color: #d63200;
  font-size: 1.2rem;
}
.end {
  color: #0d6efd;
  font-size: 1.4rem;
}
.spinner-grow {
  background: #0d6efd;
}
</style>