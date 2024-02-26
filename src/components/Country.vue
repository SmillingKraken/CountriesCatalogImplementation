<template>
  <v-card
    class="mx-auto"
    max-width="400"
  >
    <v-img
      :src="flagUrl" 
      alt="Country Flag"
      height="200px"
      cover
    ></v-img>

    <v-card-title @click="toggleDialog">
      {{ countryName }}
    </v-card-title>

    <v-card-subtitle>
      IDD: {{ idd ?? "no idd" }} <br>
      Country Code:  {{ cca2 }} , {{ cca3 }}  
    </v-card-subtitle>

    <v-card-text>
      altSpellings : 
      <span v-if="Array.isArray(altSpellings)">
        <span v-for="(altSpelling, index) in altSpellings" :key="index">
           {{ altSpelling.altName }} <span v-if="index !== altSpellings.length - 1">,</span>
        </span>
      </span> 
    </v-card-text>

    <v-card-actions>
      <v-btn
        variant="outlined"
        @click="toggleDialog"
      >
        Explore
      </v-btn>

      <v-spacer></v-spacer>

      <v-btn
        :icon="show ? 'mdi-chevron-up' : 'mdi-chevron-down'"
        @click="show = !show"
      ></v-btn>
    </v-card-actions>

    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>
        <v-card-text>
          <div v-if="Array.isArray(nativeNames)">
              <div v-for="(nName, index) in nativeNames" :key="index">
                  <div>Language Code: {{ nName.languageCode }}</div>
                  <div>Official: {{ nName.official }}</div>
                  <div>Common: {{ nName.common }}</div>
                  <br>
              </div>
          </div>
          <div v-else>
              <h2>{{ nativeNames }}</h2>
          </div>
        </v-card-text>
      </div>
    </v-expand-transition>

    <v-dialog
      v-model="dialog"
      persistent
      max-width="400"
    >
      <v-card>

        <v-img
          :src="flagUrl" 
          alt="Country Flag"
          height="200px"
          cover
        ></v-img>

        <v-card-title >
          <h2>{{ countryName }}</h2> 
          <h2> ( {{ countryDetail.name.common }} )</h2>
        </v-card-title>

        <v-card-subtitle>
          <p>Official Name: {{ countryDetail.name.official }}</p>
          <p>Capital: {{ countryDetail.capital[0] }}</p>
          <p>Area: {{ countryDetail.area }} km²</p>
          <p>Population: {{ countryDetail.population }}</p>
          <p>Region: {{ countryDetail.region }}</p>
          <p>Start of Week: {{ countryDetail.startOfWeek }}</p>
          <p>Status: {{ countryDetail.status }}</p>
          <p>Subregion: {{ countryDetail.subregion }}</p>
        </v-card-subtitle>

        <ObjectLayerOne
          :objects = countryDetail.languages
        >
          Languages:
        </ObjectLayerOne>

        <ArrayLayerOne
          :array = countryDetail.altSpellings
        >
          AltSpellings:
        </ArrayLayerOne>

        <ArrayLayerOne
          :array = countryDetail.borders
        >
          Border with:
        </ArrayLayerOne>

        <ArrayLayerOne
          :array = countryDetail.capital
        >
          Capital City:
        </ArrayLayerOne>

        <v-card-text>

          <h3>with Coordinates:</h3>
          <p>Latitude: {{ countryDetail.capitalInfo.latlng[0] }}</p>
          <p>Longitude: {{ countryDetail.capitalInfo.latlng[1] }}</p>

        </v-card-text>

        <v-card-text>

          <div v-if="countryDetail.languages">
            <h3>Car Details: </h3>
            <ul>
              <li>Side: {{ countryDetail.car.side }} </li>
              <li v-for="(sign, index) in countryDetail.car.signs" :key="index">{{ sign }}</li>
            </ul>
          </div>
          
        </v-card-text>



        <v-card-text>

            <h3>Short Codes: </h3>
            <ul>
              <li>cca2 : {{ countryDetail.cca2 ?? "null"}}</li>
              <li>cca3 : {{ countryDetail.cca3 ?? "null"}}</li>
              <li>ccn3 : {{ countryDetail.ccn3 ?? "null"}}</li>
              <li>cioc : {{ countryDetail.cioc ?? "null" }}</li>
            </ul>

        </v-card-text>
        
        <ArrayLayerOne
          :array = countryDetail.continents
        >
          Continents:
        </ArrayLayerOne>

        <v-card-text>

          <h3>Currencies : </h3>
          <ul>
            <li v-for="(currency, index) in countryDetail.currencies" :key="index">
              currency : {{ index }} , Fullname : {{ currency.name }}, symbol : {{ currency.symbol }}
            </li>
          </ul>

        </v-card-text>

        <v-card-text>

          <h3>Demonyms : </h3>
          <ul>
            <li v-for="(demonym, index) in countryDetail.demonyms" :key="index">
              {{ index }}
              <p>Female: {{ demonym.f }}</p>
              <p>Male: {{ demonym.m }}</p>
            </li>
          </ul>

        </v-card-text>

        <Translation
          :array = countryDetail.translations
        >
          Country Name Translations:
        </Translation>

        <v-card-text>
          <h3>Country Name Translations:</h3>
          <ul>
            <li v-for="(translation, lang) in countryDetail.translations" :key="lang">
              {{ lang.toUpperCase() }}: {{ translation.official }} / {{ translation.common }}
            </li>
          </ul>
        </v-card-text>

        <ArrayLayerOne
          :array = countryDetail.timezones
        >
          Timezones:
        </ArrayLayerOne>

        <v-card-actions>
          <v-btn 
            variant="outlined"
            :href="countryDetail.maps.googleMaps" 
            color="primary" 
            text 
          >
            Google Maps
          </v-btn>

          <v-spacer></v-spacer>

          <v-btn 
            variant="outlined"
            :href="countryDetail.maps.openStreetMaps" 
            color="primary" 
            text 
          >
            OpenStreetMap
          </v-btn>
          
          <v-spacer></v-spacer>
          
          <v-btn
            variant="outlined"
            color="primary"
            text
            @click="dialog = false"
          >
            Close
          </v-btn>

        </v-card-actions>
      </v-card>
    </v-dialog>

  </v-card>
</template>

<script>
import axios from "axios";
import ArrayLayerOne from "./ArrayLayerOne.vue";
import ObjectLayerOne from "./ObjectLayerOne.vue";

export default {
  components: {
    ArrayLayerOne,
    ObjectLayerOne
},
  data: () => ({
    show: false,
    dialog: false,
    countryDetail: null,
  }),
  props: {
      flagUrl: {
          type: String,
          required: true,
      },
      countryName: {
          type: String,
          required: false,
      },
      cca2: {
          type: String,
          required: false,
      },
      cca3: {
          type: String,
          required: false,
      },
      nativeNames: {
          type: [Array, String],
          required: false,
      },
      altSpellings: {
          type: Array,
          required: false,
      },
      idd: {
          type: String,
          required: false,
      },
  },
  methods: {
    toggleDialog() {
      this.dialog = !this.dialog;
      if (this.dialog) {
        this.fetchCountryDetail();
      }
    },
    fetchCountryDetail() {
      axios
        .get("https://restcountries.com/v3.1/name/" + this.countryName)
        .then((response) => {
          this.countryDetail = response.data[0];
          console.log(this.countryDetail);
        })
        .catch((error) => console.error("Error:", error));
    }
  },
  mounted() {
    this.fetchCountryDetail()
  }
};
</script>