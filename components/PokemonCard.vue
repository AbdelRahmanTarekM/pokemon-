<template>
  <!-- <nuxt-link to="/#"> -->
    <v-card max-width="300" outlined tile>
      <v-list-item three-line>
        <v-list-item-content>
          <v-list-item-title
            class="headline mb-1"
          >{{ pokeName }}</v-list-item-title>
          <v-list-item-subtitle>
            Height: {{height}} m.
            <br />
            Weight: {{weight}} lbs.
          </v-list-item-subtitle>
        </v-list-item-content>

          <img :src="pokeImage" />
        <!-- <v-list-item-avatar tile size="80">
        </v-list-item-avatar> -->
      </v-list-item>
    </v-card>
  <!-- </nuxt-link> -->
</template>

<script>
import Axios from 'axios';
export default {
  props: ["pokeName"],
  data() {
    return {
      pokeImage: null,
      height: null,
      weight: null
    };
  },
  mounted() {
    Axios.get("https://pokeapi.co/api/v2/pokemon/" + this.pokeName).then(
      response => {
        this.pokeImage = response.data.sprites.front_default;
        this.height = response.data.height;
        this.weight = response.data.weight;
        // console.log(this.pokeImage)
      }
    );
  }
};
</script>