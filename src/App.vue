<template>
  <div class="app">
      <div class="header container h-100 p-5">
        <h1 class="mb-5">Weather App</h1>
        <div class="d-flex justify-content-center h-100">
          <div class="searchbar w-50 mx-2">
            <input type="text" class="input form-control" ref="inputCity" placeholder="Enter a City">
          </div>
          <button class="btn-search btn btn-primary" @click="handleCity"><i class="fas fa-search"></i></button>
        </div>
      </div>
  </div>
  <Weather :city="city"/>
</template>

<script>
import Weather from './components/Weather.vue';


export default {
  name: 'App',
  components: {
    Weather
  },
  data () {
    return {
      city: '',
    }
  },
  methods: {
    handleCity() {
        let valueCity = this.$refs.inputCity.value;
        this.city = valueCity;
    }
  }
}

</script>

<style>

body {
  background-color: #121212 !important;
}

.header {
  background-color: #212730;
  color: #fff;
  border-radius: 5px;
  text-align: center;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  margin-top: 5rem;
}

</style>
