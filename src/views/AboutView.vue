<template>
  <div class="about d-flex flex-column gap-5 mt-5" style="height: 100vh;">
    <div>
      <button @click="goBack" class="shadow-bottom d-flex align-items-center gap-1 px-4 py-2" :class="theme === 'light' ? 'lightmode-el' : 'darkmode-el'" style="border: none; font-size: 14px; border-radius: 4px;">
      <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24"><path fill="currentColor" d="m9 18l-6-6l6-6l1.4 1.4L6.8 11H21v2H6.8l3.6 3.6z"/></svg>
      Back
      </button>
    </div>
    <div v-if="item" class="about " >
      <!-- <h1>HELLO</h1> -->
      <div class="flex-cont">
        <img :src="item.flags.png" alt="Flag" style="width: 90%; max-width: 600px; height: 310px; ">
      </div>
      <div class="row  mt-2 flex-cont" >
        <span class="fw-6">{{ item.name.common }}</span>
        <div class="about" style="font-size: 12px;">
          <div class="row gap-1 " >
            <span>Native Name: {{ item.name.nativeName }}</span>
            <span>Population: {{ item.population }}</span>
            <span>Region: {{ item.region }}</span>
            <span>Sub Region: {{ item.subregion }}</span>
            <span>Capital: {{ item.capital }}</span>
          </div>
          <div class="row">
            <span>Top Level Domain: {{ item.tld }}</span>
            <span>Currencies: {{ item.currencies }}</span>
            <span>Languages: {{ item.languages }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  // name: "About",
  data() {
    return {
      item: null
    }
  },
  props: {
    theme: String,
    // item: Object,
  },
  methods: {
    goBack() {
      this.$router.push({ name: 'home'})
      // this.$router.go(-1);
    }
  },
  mounted() {
    // console.log(this.$route.query.item);
    // this.item = JSON.parse(this.$route.query.item);
    this.item = JSON.parse(this.$route.query.item);
  }
}
</script>

<style>

.about {
  display: flex;
  gap: 50px;
}
.flex-cont {
  flex: 1;
}

@media (max-width: 500px) {
  .about {
    display: block;
  }
}

</style>
