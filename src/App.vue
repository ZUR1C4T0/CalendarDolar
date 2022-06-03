<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row justify="center">
          <v-col cols="auto">
            <h2>Calendario del precio del Dólar</h2>
          </v-col>
        </v-row>
        <v-row justify="center">
          <v-col sm="8" md="6">
            <Calendar v-model="date" :maxDate="maxDate" @change="getDolar" />
          </v-col>
        </v-row>
        <v-row justify="center">
          <v-col sm="8" md="6">
            <Price :valor="valor" />
          </v-col>
        </v-row>
      </v-container>
    </v-main>

    <v-dialog v-model="loading" hide-overlay persistent width="300">
      <v-card color="grey" dark>
        <v-card-text>
          Cargando...
          <v-progress-linear indeterminate color="white" class="mt-1" />
        </v-card-text>
      </v-card>
    </v-dialog>
    <!-- --- -->
  </v-app>
</template>

<script>
  import Price from "./components/Price.vue";
  import Calendar from "./components/Calendar.vue";
  import axios from "axios";
  import { mapMutations, mapState } from "vuex";

  export default {
    name: "App",

    components: { Price, Calendar },

    data() {
      return {
        date: new Date(new Date(Date.now() - new Date().getTimezoneOffset() * 60000))
          .toISOString()
          .substr(0, 10),

        maxDate: new Date(new Date(Date.now() - new Date().getTimezoneOffset() * 60000))
          .toISOString()
          .substr(0, 10),

        valor: null
      };
    },

    computed: { ...mapState(["loading"]) },

    methods: {
      async getDolar(day) {
        day = day.split("-").reverse().join("-");
        try {
          this.toggleLoading();
          let request = await axios.get(`https://mindicador.cl/api/dolar/${day}`);
          this.valor = await request.data.serie[0].valor;
        } catch (error) {
          this.valor = "Sin resultado";
        } finally {
          this.toggleLoading();
        }
      },

      ...mapMutations(["toggleLoading"])
    },

    created() {
      this.getDolar(this.date);
    }
  };
</script>
