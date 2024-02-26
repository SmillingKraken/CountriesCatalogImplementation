<template>
    <v-card-text>
        <h3><slot></slot></h3>
        <ul v-if="array && array.length > 0" >
            <li v-for="index in array" :key="index">
                {{ index }}
            </li>
        </ul>
        <p v-else>No Data available</p>
    </v-card-text>
</template>

<script>
    export default {
        props: {
            array: {
                type: Array,
                required: true,
            },
        },
    }
</script>