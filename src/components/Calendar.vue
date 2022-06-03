<template>
  <v-card>
    <v-date-picker
      v-model="date"
      full-width
      locale="es-co"
      min="1984"
      :max="maxDate"
      @change="updateDate"
    />
  </v-card>
</template>

<script>
  export default {
    name: "Calendar",

    props: ["value", "maxDate"],

    computed: {
      date: {
        get() {
          return this.value;
        },
        set(newValue) {
          this.$emit("input", newValue);
        }
      }
    },

    methods: {
      updateDate(newValue) {
        this.$emit("change", newValue);
      }
    },

    BeforeUpdated() {
      this.getDolar(this.date);
    }
  };
</script>
