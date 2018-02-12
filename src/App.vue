<template>
  <div class="pokemon-list">
    <div class="field pokemon-name has-addons">
      <div class="control is-expanded">
        <input v-model="pokemonName" class="input" type="text" name="pokemon-name" id="pokemon-name"
               @focus="clearPokemonNotFound">
      </div>
      <div class="control">
        <a class="button is-info" @click="addPokemon">
          Add
        </a>
      </div>
    </div>
    <div v-for="pokemon in pokemons">
      <Pokemon v-bind:pokemon="pokemon"/>
    </div>
    <div v-if="pokemons.length === 0">
      <div class="notification">
        There are no pokemonts yet! Train hard and catch all of them!
      </div>
    </div>
    <div v-if="pokemonNotFound">
      <div class="notification">
        There is no pokemon with name: {{ pokemonName }}
      </div>
    </div>
  </div>
</template>

<script>
import {Pokedex} from 'pokeapi-js-wrapper';
import Pokemon from './Pokemon.vue';

var options = {
  protocol: 'https',
  cache: false
};

var P = new Pokedex(options);

export default {
  name: 'app-component',
  components: {Pokemon},
  data() {
    return {
      pokemonName: null,
      pokemons: [],
      pokemonNotFound: false
    }
  },
  methods: {
    addPokemon(evt) {
      P.getPokemonByName(this.pokemonName).then((response) => {
        var pokemon = {name: response.name, sprites: response.sprites};
        P.getPokemonSpeciesByName(this.pokemonName).then((response) => {
          pokemon.description = response.flavor_text_entries[2].flavor_text;

          this.pokemons.push(pokemon);
          this.pokemonNotFound = false;
        });
      }).catch((error) => {
        this.pokemonNotFound = true;
      });
    },
    clearPokemonNotFound() {
      this.pokemonNotFound = false;
    }
  }
}
</script>

<style scoped>
.pokemon-list {
  width: 100%;
}

.pokemon-list .field.pokemon-name {
  margin-top: 10px;
}

.pokemon-list #pokemon-name {
  width: 100%;
}

.pokemon-list .notification {
  text-align: center;
}
</style>
