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

    <v-card-title>
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

    <v-card-actions>
      <v-btn
        variant="outlined"
      >
        Explore
      </v-btn>
    </v-card-actions>

  </v-card>
</template>

<script>
    export default {
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
    };
</script>