<template>
    <div class="container-home">
        <div :class=" dark ? 'navbar-dark-mode' : 'navbar'">
          <div class="navbar-container">
            <div class="navbar-title">
                Where in the world ?
            </div>

            <div class="navbar-dark" @click="changeMode()">
                <div class="navbar-dark-icon"><i class="far fa-moon"></i></div>
                <div class="navbar-dark-text">Dark Mode </div>
            </div>
          </div>
        </div>
        
        <div :class="dark ? 'home-dark' : 'home'">
          <div class="search">
              <div class="input-search">
                <i class="fas fa-search"></i>
                <input
                  v-model="search"
                  @change="searchCountries(this.search)"
                  class="input"
                  type="search"
                  placeholder="Search for a country..." />
              </div>
              <div class="select-search">
                <select v-model="region" @change="filterCountries(this.region)" class="input">
                  <option value="">Filter by Region</option>
                  <option value="africa">Africa</option>
                  <option value="americas">Americas</option>
                  <option value="asia">Asia</option>
                  <option value="europe">Europe</option>
                  <option value="oceania">Oceania</option>
                </select>
              </div>
          </div>

          <div class="countries-list">
            <div class="country-card" v-for="country in countries" :key="country.name">
              <div class="country-flag">
                <img class="flag" :src="country.flags.png" alt="flag" />
              </div>
              <router-link :to="'/country/' + country.name.common" class="router">
                <div class="country-info">
                  <div class="country-name">{{country.name.common}}</div>
                  <div class="country-category">
                    <div class="category-title">Population: </div>
                    <div class="category-subtitle">{{country.population.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")}}</div>
                  </div>
                  <div class="country-category">
                    <div class="category-title">Region: </div>
                    <div class="category-subtitle">{{country.region}}</div>
                  </div>
                  <div class="country-category">
                    <div class="category-title">Capital: </div>
                    <div class="category-subtitle" v-for="capital in country.capital" :key="capital">{{capital}}</div>
                  </div>
                </div>
              </router-link>
            </div>
          </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Index',
  components: {
      
  },
  props: {
    msg: String
  },
  data () {
    return {
      countries: [],
      region: '',
      search: '',
      dark: false
    }
  },
  created () {
    this.getCountries();
  },
  methods: {
    getCountries () {
      return new Promise((resolve)=>{
            axios.get('https://restcountries.com/v3.1/all')
            .then((response) =>{
              this.countries = response.data;
              resolve(response.data);
            })   
        })
    },

    filterCountries (region) {
      if(region === '') {
        this.getCountries();
      } else {
        return new Promise((resolve)=>{
            axios.get('https://restcountries.com/v3.1/region/' + region)
            .then((response) =>{
              this.countries = response.data;
              resolve(response.data);
            })   
        })
      }
    },

    searchCountries (search) {
      if(search === '') {
        this.getCountries();
      } else {
        return new Promise((resolve)=>{
            axios.get('https://restcountries.com/v3.1/name/' + search)
            .then((response) =>{
              this.countries = response.data;
              resolve(response.data);
            })   
        })
      }
    },

    changeMode () {
      this.dark = !this.dark;
    }
  }
}
</script>