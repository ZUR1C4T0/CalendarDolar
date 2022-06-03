<template>
  <v-card color="green" elevation="6" dark>
    <v-card-text class="display-1 text-center">
      {{ typeof valor === "number" ? `1 USD = ${valor} CLP` : valor }}
    </v-card-text>
  </v-card>
</template>

<script>
  export default {
    name: "Price",

    props: ["valor"]
  };
</script>
