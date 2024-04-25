<template>
  <div class="home">
    <div class="d-flex flex-wrap justify-content-between py-5">
      <div>
        <div class=" position-relative px-4 py-2 shadow-bottom" :class="theme === 'light' ? 'lightmode-el' : 'darkmode-el'" style="border-radius: 4px;">
          <svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24" class="position-absolute " style="top: 40%;"><path fill="currentColor" d="M15.5 14h-.79l-.28-.27A6.471 6.471 0 0 0 16 9.5A6.5 6.5 0 1 0 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5S14 7.01 14 9.5S11.99 14 9.5 14"/></svg>
          <input type="search"  name="" id="" placeholder="Search for a country" class="mx-4" style="border: none; background-color: transparent; outline: none;" >
        </div>
      </div>
      <div>
        <select name="filter" id="filter" placeholder="Filter by region">
          <option value="Africa">Africa</option>
          <option value="Asia">Asia</option>
          <option value="Europe">Europe</option>
        </select>
      </div>

    </div>
    <div class="">
      <div class="parent" >
          <div v-for="item in items" :key="item.id" @click="goToAbout(item)" class="item overflow-hidden shadow-bottom" style=" border-radius: 4px;" :class="theme === 'light' ? 'lightmode-el' : 'darkmode-el'">
            <!-- <RouterLink :to="{ name: 'about', params: { id: item.id } }" style="text-decoration: none; color: inherit;"> -->
              <div>
                <img :src="item.flags.png" alt="Flag" style="width: 100%; height: 120px; ">
              </div>
              <div class="row py-2 px-3 gap-2 mt-2">
                <span class="fw-6">{{ item.name.common }}</span>
                <div class="row gap-1 " style="font-size: 12px;">
                  <span v-if="false">Native Name: {{ item.name.nativeName }}</span>
                  <span>Population: {{ item.population }}</span>
                  <span>Region: {{ item.region }}</span>
                  <span v-if="false">Sub Region: {{ item.subregion }}</span>
                  <span>Capital: {{ item.capital }}</span>
                  <span v-if="false">Top Level Domain: {{ item.tld }}</span>
                  <span v-if="false">Currencies: {{ item.currencies }}</span>
                  <span v-if="false">Languages: {{ item.languages }}</span>
                </div>
              </div>
          </div>
        <!-- </RouterLink> -->
      </div>
    </div>

    <!-- <RouterView /> -->
  </div>
</template>

<script>
import axios from 'axios';
import cache from '../cache';

export default {
  props: {
    theme: String,
  },
  data() {
    return {
      items: [],
      // searchQuery: '',
      // loading: true,
    }
  },
  // computed: {
  //   filteredItems() {
  //     return this.item.filter(item =>
  //     item.name.common.toLowerCase().includes(this))
  //   }
  // },
  methods: {
    goToAbout(item) {
      this.$router.push({
        name: 'about',
        params: { id: item.id},
        query: { item: JSON.stringify(item)}
      })
    }
  },
  mounted() {
    if (cache.homeData) {
      this.items = cache.homeData
    } else {
      axios
      .get("https://restcountries.com/v3.1/all")
      .then (response => {
          console.log(response)
          this.items = response.data;
          cache.homeData = response.data;
        }
      )
      .catch (error => {console.log(error);
      })
    }


  }
}


</script>

<style>

.parent {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); /* Adjust the width as needed */
  grid-auto-rows: 1fr; /* Equal height for each row */
  gap: 50px; /* Adjust the gap between child elements as needed */
}

::placeholder {
  font-size: 11px;
}

.item {
  /* Your existing styles for items */
  transition: transform 0.3s; /* Add transition for smooth scaling */

}

/* Apply scaling effect on hover */
.item:hover {
  transform: scale(1.07); /* Adjust the scale factor as needed */
}




</style>

