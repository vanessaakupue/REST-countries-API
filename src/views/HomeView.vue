<template>
  <div class="home" style="min-height: 100vh; padding-top: 50px;">
    <div class="d-flex flex-wrap justify-content-between pt-5">
      <!-- Search bar -->
      <div>
        <div class="d-flex align-items-center position-relative px-4 py-2 shadow-bottom" :class="theme === 'light' ? 'lightmode-el' : 'darkmode-el'" style="border-radius: 4px;">
          <svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24" class="position-absolute " style="top: 40%;"><path fill="currentColor" d="M15.5 14h-.79l-.28-.27A6.471 6.471 0 0 0 16 9.5A6.5 6.5 0 1 0 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5S14 7.01 14 9.5S11.99 14 9.5 14"/></svg>
          <input type="search" v-model="search" placeholder="Search for a country" class="mx-4" style="border: none; background-color: transparent; outline: none;" >
        </div>
      </div>
      <!-- Filter by Region -->
      <div class="filter-container shadow-bottom p-0" :class="theme === 'light' ? 'lightmode-el' : 'darkmode-el'">
        <button class="drop-btn gap-5 mx-3 my-2" :class="theme === 'light' ? 'lightmode-el' : 'darkmode-el'"  @click="toggleDropdown" >{{ selectedRegion ? selectedRegion : "Filter by Region" }}
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24"><path fill="currentColor" d="m12 15l-4.243-4.242l1.415-1.414L12 12.172l2.828-2.828l1.415 1.414z"/></svg>
        </button>
        <ul v-show="showDropdown" class="dropdownmenu shadow-bottom">
          <li @click="filterByRegion('')">All</li>
          <li class="py-1" v-for="region in regions" :key="region" @click="filterByRegion(region)" >
            {{ region }}
          </li>
        </ul>
      </div>

    </div>

    <div v-if="items.length" class="d-flex justify-content-center py-3" style=" font-size: 12px;" >
        {{ filteredItems.length }} {{ filteredItems.length === 1 ? 'country' : 'countries' }}
    </div>
    <!-- List of countries -->
    <div class="">
      <div v-if="items.length" class="parent pb-5" >
          <div v-for="item in filteredItems" :key="item.id" @click="goToAbout(item)" class="item overflow-hidden shadow-bottom" style=" border-radius: 4px; cursor: pointer;" :class="theme === 'light' ? 'lightmode-el' : 'darkmode-el'">
              <div>
                <img :src="item.flags.png" alt="Flag" style="width: 100%; height: 120px; ">
              </div>
              <div class="row py-2 px-3 gap-2 mt-2">
                <span class="fw-6">{{ item.name.common }}</span>
                <div class="row gap-1 " style="font-size: 12px;">
                  <span v-if="item.capital" class="fw-6" >Capital:
                    <span style="opacity: 0.7;" > {{ cleanUpValue(item.capital) }} </span>
                  </span>
                  <span v-if="item.population" class="fw-6" >Population:
                    <span style="opacity: 0.7;" > {{ item.population.toLocaleString() }} </span>
                  </span>
                  <span v-if="item.region" class="fw-6" >Region:
                    <span style="opacity: 0.7;" > {{ item.region }} </span>
                  </span>

                </div>
              </div>
          </div>
      </div>

      <div v-else >
        Countries Loading ...
      </div>
    </div>
  </div>
</template>

<script>


export default {
  props: {
    theme: String,
  },
  data() {
    return {
      search: this.$route.query.search || "",
      items: [],
      showDropdown: false,
      regions: [],
      selectedRegion: this.$route.query.region || '',
    }
  },
  methods: {
    goToAbout(item) {
      this.$router.push({
        name: 'about',
        params: { name: item.name.common},
        // query: { search: this.search, region: this.selectedRegion }
      })
    },
    cleanUpValue(value) {
      if (typeof value === 'object') {
        const values = Object.values(value);
        const stringValues = values.filter(v => typeof v === 'string');
        return stringValues.join(', ');
      } else {
        return value;
      }
    },
    getNativeName(item) {
      if (item.name.nativeName && Object.values(item.name.nativeName).length > 0) {
        return Object.values(item.name.nativeName)[0].official;
      } else {
        return "N/A";
      }
    },
    getCurrencyNames(currencies) {
      if (currencies) {
        const currencyNames = Object.values(currencies).map(currency => currency.name);
        return currencyNames.join(', ');
      } else {
        return "N/A";
      }
    },
    toggleDropdown() {
      this.showDropdown = !this.showDropdown
      // console.log("dropdown:", this.showDropdown)
    },
    filterByRegion(region) {
      this.selectedRegion = region;
      this.showDropdown = false;
      this.updateQuery();
    },
     updateQuery() {
      const query = {};
      if (this.search) query.search = this.search;
      if (this.selectedRegion) query.region = this.selectedRegion;

      this.$router.push({
        query
      });
    },
    extractRegions() {
      const uniqueRegions = [...new Set(this.items.map(item => item.region))];
      this.regions = uniqueRegions.filter(region => region !== '');
    },
  },
  computed: {
    filteredItems() {

      let filteredItems = this.items;

      if(this.search) {
        const searchString = this.search.toLowerCase();
        filteredItems = filteredItems.filter(item => {
          const capital = item.capital ? item.capital.join(', ').toLowerCase() : '';
          return item.name.common.toLowerCase().includes(searchString) || capital.includes(searchString)
        });
      }

      if(this.selectedRegion) {
        filteredItems = filteredItems.filter( item => item.region === this.selectedRegion)
      }
      this.updateQuery();
      return filteredItems.sort((a, b) => a.name.common.localeCompare(b.name.common));

    }
  },
  mounted() {
      fetch("https://restcountries.com/v3.1/all")
      .then (response => response.json() )
      .then (data => {
        // console.log("fetched data:", data)
        this.items = data.sort((a, b) => a.name.common.localeCompare(b.name.common));
        this.extractRegions();
      })
      .catch (error => {console.log(error);
      })
  }
}


</script>

<style>

.parent {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  grid-auto-rows: 1fr;
  gap: 50px;
}

::placeholder {
  font-size: 11px;
}

.item {
  transition: transform 0.3s;
}
.item:hover {
  transform: scale(1.07);
}

.filter-container {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 5px;
  /* width: 200px; */
  align-items: center;
  border-radius: 4px;
}

.drop-btn {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: transparent;
  border: none;
  outline: none;
  font-size: 12px;
  opacity: 0.7;
  /* margin-bottom: 5px; */
}

.dropdownmenu {
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  /* display: none; */
  margin-top: 5px;
  padding: 0.5rem;
  background-color: inherit;
  border-radius: 4px;
  /* border: 1px solid #ccc; */
  /* border-top: none; */
  list-style: none;
}

.dropdownmenu li {
  cursor: pointer;
  opacity: 0.7;
  font-size: 12px;
}

.dropdownmenu li:hover {
  opacity: 1.5;
  text-decoration: underline;
}

@media (max-width: 556px) {
  .filter-container {
    margin-top: 30px;
  }
}

/*





.dropdownmenu.show {
  display: block;
} */

</style>

