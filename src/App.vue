<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/pokedex.png">
      <hr>
      <h4 class="is-size-4">Pokédex</h4>
      <input type="text" placeholder="Buscar pokémon pelo nome" v-model="busca" class="input is-rounded">
      <button class="button is-fullwidth is-success" id="buscaBtn" @click="buscar">Buscar</button>
      <div v-if="filteredPokemons != undefined">
        <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <PokemonView :name="poke.name" :url="poke.url" :num="index+1"/>
      </div>
      </div>
    </div>    
  </div>
</template>

<script>
import axios from 'axios';
import PokemonView from './components/PokemonView.vue'
import { inject } from '@vercel/analytics';
 
inject();

export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=10000&offset=0").then(res => {
      this.pokemons = res.data.results;
      this.filteredPokemons = [];
    })
  },
  components: {
    PokemonView
  },
  methods: {
    buscar: function(){
      this.filteredPokemons = this.pokemons;
      if(this.busca == '' || this.busca == ' '){
        this.filteredPokemons = [];
      }else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.toLowerCase() == this.busca.toLowerCase());
      }
    }
  },
  computed: {
    // resultadoBusca: function(){
    //   if(this.busca == '' || this.busca == ' '){
    //     return this.pokemons;
    //   }else{
    //     return this.pokemons.filter(pokemon => pokemon.name == this.busca)
    //   }
    // }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#buscaBtn{
  margin-top: 2%;
}
</style>
