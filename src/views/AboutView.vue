<template>
  <div class="about d-flex flex-column gap-5 py-5" style="min-height: 100vh; margin-top: 50px;">
    <div>
      <button @click="goBack" class="shadow-bottom d-flex align-items-center gap-1 px-4 py-2" :class="theme === 'light' ? 'lightmode-el' : 'darkmode-el'" style="border: none; font-size: 14px; border-radius: 4px;">
      <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24"><path fill="currentColor" d="m9 18l-6-6l6-6l1.4 1.4L6.8 11H21v2H6.8l3.6 3.6z"/></svg>
      Back
      </button>
    </div>
    <div v-if="item" class="about " >
      <div class="flex-cont">
        <img :src="item.flags.png" alt="Flag" class="flag-sm" style="width: 100%; max-width: 600px; height: 310px; ">
      </div>
      <div class="py-4 flex-cont" >
        <div class="d-flex gap-2">
          <span class="fw-bold common-name-sm" style="font-size: 18px; letter-spacing: 2px;" >{{ item.name.common }}
          </span>
          <div @click="playPronunciation" class="d-flex align-items-end"  style="cursor: pointer;"  >
            <svg  xmlns="http://www.w3.org/2000/svg" width="25" height="25" viewBox="0 0 24 24"><path fill="currentColor" d="M19 11.975q0-2.075-1.1-3.787t-2.95-2.563q-.375-.175-.55-.537t-.05-.738q.15-.4.538-.575t.787 0Q18.1 4.85 19.55 7.063T21 11.974t-1.45 4.913t-3.875 3.287q-.4.175-.788 0t-.537-.575q-.125-.375.05-.737t.55-.538q1.85-.85 2.95-2.562t1.1-3.788M7 15H4q-.425 0-.712-.288T3 14v-4q0-.425.288-.712T4 9h3l3.3-3.3q.475-.475 1.088-.213t.612.938v11.15q0 .675-.612.938T10.3 18.3zm9.5-3q0 1.05-.475 1.988t-1.25 1.537q-.25.15-.513.013T14 15.1V8.85q0-.3.263-.437t.512.012q.775.625 1.25 1.575t.475 2"/></svg>
          </div>
        </div>
        <div class="about-row mt-3 mb-5" style="font-size: 12px;">
          <div class="row gap-2 " >
            <span class="fw-6 d-flex gap-1 flex-wrap" >Native Name:
              <span style="opacity: 0.7;" >{{ getNativeName(item) }}</span>
            </span>
            <span v-if="item.capital" class="fw-6">Capital:
              <span style="opacity: 0.7;" > {{ cleanUpValue(item.capital) }}</span>
            </span>
            <span v-if="item.population" class="fw-6">Population:
              <span style="opacity: 0.7;" >{{ item.population.toLocaleString() }}</span>
            </span>
            <span v-if="item.region" class="fw-6">Region:
              <span style="opacity: 0.7;" >{{ item.region }}</span>
            </span>
            <span v-if="item.subregion" class="fw-6">Sub Region:
              <span style="opacity: 0.7;" > {{ item.subregion }}</span>
            </span>

          </div>
          <div class="row second-row-sm">
            <span class="fw-6">Area:
              <span style="opacity: 0.7;" >{{ cleanUpValue(item.area.toLocaleString()) }} km²</span>
            </span>
            <span v-if="item.currencies" class="fw-6">Currencies:
              <span style="opacity: 0.7;" >{{ getCurrencyNames(item.currencies) }}</span>
            </span>
            <span v-if="item.languages" class="fw-6">Official {{ Object.keys(item.languages).length === 1 ? 'Language' : 'Languages' }}:
              <span style="opacity: 0.7;" > {{ cleanUpValue(item.languages) }} </span>
            </span>
          </div>
        </div>
        <!-- Border countries -->
        <div class="d-flex gap-3 align-items-center border-sm" >
          <span v-if="item.borders" class="fw-6 " style="font-size: 12px;"  >Border countries:</span>
          <div  v-if="item.borders" class="d-flex flex-wrap gap-2" >
            <span
            v-for="border in item.borders"
            :key="border" @click="goToBorderCountry(border)"
            :ref="border"
            style="font-size: 10px; border-radius: 4px; opacity: 0.7; cursor: pointer;" class="about-item shadow-bottom px-3 py-2"
            :class="theme === 'light' ? 'lightmode-el' : 'darkmode-el'" >
            {{ cleanUpValue(border) }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import tippy from 'tippy.js';
import 'tippy.js/dist/tippy.css';

export default {

  data() {
    return {
      item: null,
      items: []
    }
  },
  props: {
    theme: String,
  },
  methods: {
    goBack() {
      // this.$router.push({ name: 'home'})
      this.$router.go(-1);
    },
    playPronunciation() {
      const utterance = new SpeechSynthesisUtterance(this.item.name.common);
      speechSynthesis.speak(utterance);
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
    goToBorderCountry(cca3) {
      const borderCountry = this.items.find(item => item.cca3 === cca3);
      if (borderCountry) {
        this.$router.push({
          name: 'about',
          params: { id: borderCountry.id },
          query: { item: JSON.stringify(borderCountry) }
        })
      }
    },
    fetchAllCountries() {
      fetch("https://restcountries.com/v3.1/all")
      .then (response => response.json() )
      .then (data => {
        console.log("fetched data:", data)
        this.items = data.sort((a, b) => a.name.common.localeCompare(b.name.common));
        this.initializeTooltips();
      })
      .catch (error => {console.log(error);
      })
    },
    initializeItem() {
      this.item = JSON.parse(this.$route.query.item);
      this.scrollToTop();
    },
    scrollToTop() {
      window.scrollTo(0, 0);
    },
    initializeTooltips() {
      this.$nextTick(() => {
        this.item.borders.forEach(border => {
          const country = this.items.find(item => item.cca3 === border);
          const commonName = country.name.common ;
          const tippyTheme = this.theme === 'light' ? 'light-tippy' : 'dark-tippy';
          tippy(this.$refs[border], {
            content: commonName,
            theme: tippyTheme,
            duration: 500,
          });
        });
      });
    }
  },
  watch: {
    '$route.query.item': 'initializeItem'
  },
  mounted() {
    if (this.$route.query.item) {
      this.item = JSON.parse(this.$route.query.item);
    }
    this.fetchAllCountries();
    this.initializeItem();
  },
  updated() {
    this.initializeTooltips();
    this.scrollToTop();
  }
}
</script>

<style>
.tippy-box[data-theme~='dark-tippy'] {
  background-color: var(--white);
  color: var(--light-mode-text);
  box-shadow: 0 0 7px rgba(0, 0, 0, 0.1);
}
.tippy-box[data-theme~='dark-tippy']> .tippy-arrow::before {
  background-color: transparent;
  color: var(--white);
}

.tippy-box[data-theme~='light-tippy'] {
  background-color: var(--dark-mode-background);
  color: var(--white);
  box-shadow: 0 0 7px rgba(0, 0, 0, 0.1);
}
.tippy-box[data-theme~='light-tippy']> .tippy-arrow::before {
  background-color: transparent;
  color: var(--dark-mode-background);
}
.tippy-box[data-theme~='dark-tippy'] .tippy-content,
.tippy-box[data-theme~='light-tippy'] .tippy-content {
  font-size: 12px;
}
.about {
  display: flex;
  gap: 50px;
}
.flex-cont {
  flex: 1;
}
.about-row {
  display: flex;
  gap: 10px;
}

.about-item {
  transition: transform 0.3s;
}
.about-item:hover {
  transform: scale(1.10);
}

@media (max-width: 600px) {
  .about {
    display: block;
  }
  .flag-sm {
    max-height: 220px;
  }
  .common-name-sm {
    margin-top: 30px !important;
  }
  .second-row-sm {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-top: 30px;
  }
  .border-sm {
    display: flex;
    flex-direction: column;
    gap: 10px;
    align-items: flex-start !important;
  }
  .about-row {
    display: block;
  }

}

</style>
