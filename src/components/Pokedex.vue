<template>
  <div class="column is-half is-offset-one-quarter">
      <h4 class="is-size-4">Pokedex</h4>
      <input type="text" placeholder="Buscar pokemon pelo nome" v-model="busca" class="input is-rounded">
      <hr>
      <div v-for="(poke) in resultadoBusca" :key="poke.name">
      <Pokemon :name="poke.name" :url="poke.url"/>
    </div>
    </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './Pokemon';

export default {
    name: 'Pokedex',
    data(){
    return{
    pokemons: [],
    busca: ''
    }
  },
  created: function(){
  axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res =>
    {
      //console.log(res.data.results);
      this.pokemons = res.data.results;
      //console.log(this.pokemons);
    });
  },
  components:{
    Pokemon
  },
  computed: {
    resultadoBusca: function(){
      if(this.busca == '' || this.busca == ' '){
        return this.pokemons;
      }
      else{
        return this.pokemons.filter(pokemon => pokemon.name.match(this.busca));
      }
    }
  }
}

</script>

<style>

</style>