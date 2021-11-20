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

        <div :class="dark ? 'details-dark' : 'details'">
            <div class="details-container">
                <div class="country" v-for="item in country" :key="item.name">
                    <div class="back-button">
                        <router-link to="/" class="router">
                            <div>
                                Back
                            </div>
                        </router-link>
                    </div>

                    <div class="info">
                        <div class="info-flag">
                            <img class="flag" :src="item.flags.svg" alt="flag" />
                        </div>

                        <div class="info-list">
                            <div class="list-title">{{item.name.common}}</div>
                            <div class="list-others">
                                <div class="category">
                                    <div class="category-title">Official Name: </div>
                                    <div class="category-subtitle">{{item.name.official}}</div>
                                </div>
                                <div class="category">
                                    <div class="category-title">Top Level Domain: </div>
                                    <div class="category-subtitle" v-for="tld in item.tld" :key="tld">{{tld}}</div>
                                </div>
                                <div class="category">
                                    <div class="category-title">Population: </div>
                                    <div class="category-subtitle">{{item.population.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")}}</div>
                                </div>
                                <div class="category">
                                    <div class="category-title">Currencies: </div>
                                    <div class="category-subtitle">{{item.currencies}}</div>
                                </div>
                                <div class="category">
                                    <div class="category-title">Region: </div>
                                    <div class="category-subtitle">{{item.region}}</div>
                                </div>
                                <div class="category">
                                    <div class="category-title">Languages: </div>
                                    <div class="category-subtitle">{{item.languages}}</div>
                                </div>
                                <div class="category">
                                    <div class="category-title">Sub Region: </div>
                                    <div class="category-subtitle">{{item.subregion}}</div>
                                </div>
                                <div class="category">
                                    <div class="category-title">Capital: </div>
                                    <div class="category-subtitle" v-for="capital in item.capital" :key="capital">{{capital}}</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'CountryDetails',
//   components: {
//       NavbarHome
//   },
  props: {
  },
  data () {
    return {
      country: [],
      dark: false,
      code: this.$route.params.name
    }
  },
  created () {
    this.getCountryById();
  },
  methods: {
    getCountryById () {
      return new Promise((resolve)=>{
            axios.get('https://restcountries.com/v3.1/name/' + this.code)
            .then((response) =>{
                console.log('data', response.data);
              this.country = response.data;
              resolve(response.data);
            })   
        })
    },
  }
}
</script>