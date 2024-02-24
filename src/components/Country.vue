<template>
    <div class="col-md-4">
        <div class="card ">
            <img :src="flagUrl" alt="Country Flag"/>
            <div class="card-body">
                <slot></slot>
                <div v-if="Array.isArray(nativeNames)">
                    <div v-for="(nName, index) in nativeNames" :key="index">
                        <h2>Native Name key: {{ nName.languageCode }}</h2>
                        <h2>Official: {{ nName.official }}</h2>
                        <h2>Common: {{ nName.common }}</h2>
                    </div>
                </div>
                <div v-else>
                    <h2>{{ nativeNames }}</h2>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
    props: {
        flagUrl: {
        type: String,
        required: true,
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
      type: Object,
      required: false,
    },
    },
    };
</script>