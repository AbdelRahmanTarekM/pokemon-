<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6 class="flex-container">
      <v-card
        color="#121212"
        class="d-flex flex-nowrap py-2 pa-6 align-self-center"
        v-for="poke in pokes"
        :key="poke.name"
      >
        <PokemonCard v-bind:pokeName="poke.name" />
      </v-card>
      <v-card-actions class="justify-center">
        <v-btn class="mx-4" flat v-on:click="previousPage" :disabled="!previous">Previous</v-btn>
        <span>{{page}}</span>
        <v-btn class="mx-4" flat v-on:click="nextPage" :disabled="!next">Next</v-btn>
      </v-card-actions>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from "axios";
import Logo from "~/components/Logo.vue";
import VuetifyLogo from "~/components/VuetifyLogo.vue";
import PokemonCard from "~/components/PokemonCard";

export default {
  components: {
    Logo,
    VuetifyLogo,
    PokemonCard
  },
  data() {
    return {
      pokes: null,
      previous: null,
      next: null,
      page: 1
    };
  },
  created() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?offset=0&limit=15")
      .then(response => {
        this.pokes = response.data.results;
        this.previous = response.data.previous;
        this.next = response.data.next;
        // console.log(this.pokes)
      });
    // console.log(this.pokes);
  },
  methods: {
    previousPage() {
      axios.get(this.previous).then(response => {
        this.pokes = response.data.results;
        this.previous = response.data.previous;
        this.next = response.data.next;
        this.page--;
      });
    },
    nextPage() {
      axios.get(this.next).then(response => {
        this.pokes = response.data.results;
        this.previous = response.data.previous;
        this.next = response.data.next;
        this.page++;
      });
    }
  }
};
</script>

<style>
.flex-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  /* flex-direction: row; */
}
</style>