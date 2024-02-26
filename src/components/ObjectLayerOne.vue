<template>
    <v-card-text>
        <h3><slot></slot></h3>
        <ul>
            <li v-for="(language, code) in objects" :key="code">
                {{ language }} ({{ code }})
            </li>
        </ul>
    </v-card-text>
</template>

<script>
    export default {
        props: {
            objects: {
                type: Object,
                required: true,
            },
        },
    }
</script>