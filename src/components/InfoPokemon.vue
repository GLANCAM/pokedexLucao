<template>
    <div>
        
    <div class="column is-half is-offset-one-quarter">
        <router-link :to="{name: 'Pokedex'}"><button class="button is-info is-rounded">Pokedex</button></router-link>
        <hr>
        <h4 class="is-size-4">{{this.$route.params.pokeName | upper}}</h4>
        <hr>
        <table class="table is-fullwidth">
            <thead>
                <tr>
                <th><abbr title="NAME">Inicial</abbr></th>
                <th v-if="Evolution1.length > 0"><abbr title="EVO1">Evolução 1</abbr></th>
                <th v-if="Evolution2.length > 0"><abbr title="EVO2">Evolução 2</abbr></th>
                </tr>
            </thead>
            <tbody>
            <td class="center-textt">
                
                <tr v-for="(poke,index) in Evolution0" :key="index" class="b-1px">{{ poke | upper }}
                <br> 
                <router-link :to="{name: 'InfoPokemon', params: {pokeName: poke}}"><img :src="Img0[index]" alt="Falha no carregamento da imagem"></router-link></tr>
                
            </td>
            <td v-if="Evolution1.length > 0">
                <tr v-for="(poke,index) in Evolution1" :key="index" class="b-1px">{{ poke | upper }} 
                <br> 
                <router-link :to="{name: 'InfoPokemon', params: {pokeName: poke}}"><img :src="Img1[index]" alt="Falha no carregamento da imagem"></router-link></tr>
            </td>
            <td v-if="Evolution2.length > 0">
                <tr v-for="(poke,index) in Evolution2" :key="index" class="b-1px">{{ poke | upper }} 
                <br> 
                <router-link :to="{name: 'InfoPokemon', params: {pokeName: poke}}"><img :src="Img2[index]" alt="Falha no carregamento da imagem"></router-link></tr>
            </td>
            </tbody>
        </table>
        <div class="box">
        <h3>Altura: {{ PokeCarac.Altura }} cm</h3>
        <h3>Peso: {{ PokeCarac.Peso/10 }} kg</h3>
        </div>

        <table v-if="PokeMoves.length > 0" class="table is-fullwidth is-bordered">
            <thead>
                <tr>
                <th><abbr title="Move">Movimentos Possíveis</abbr></th>
                </tr>
            </thead>
            <tfoot>
                <tr>
                <th><abbr title="Move">Movimentos Possíveis</abbr></th>
                </tr>
            </tfoot>
            <tbody>
                <td>
                    <tr class="b-1px" v-for="(poke,index) in PokeMoves" :key="index">{{poke.move.name}}</tr>
                </td>
            </tbody>
        </table>
    </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: 'InfoPokemon',
     created: function(){
        axios.get("https://pokeapi.co/api/v2/pokemon/"+this.$route.params.pokeName).then(res =>{
            this.PokeMoves = res.data.moves;
            this.PokeCarac.Peso = res.data.weight;
            this.PokeCarac.Altura = res.data.height;
            var ID = res.data.id;
            axios.get("https://pokeapi.co/api/v2/pokemon-species/"+ID).then(res2 =>{
                this.PokeEvoChain = res2.data;
                if(this.PokeEvoChain.evolves_from_species != null){
                    var URL = this.PokeEvoChain.evolves_from_species.url;
                    axios.get(URL).then(res3 => {
                        this.PokeEvo0 = res3.data;
                        if(this.PokeEvo0.evolves_from_species == null){
                            this.Evolution0.push(this.PokeEvo0.name);
                        }
                        else{
                            this.Evolution0.push(this.PokeEvo0.evolves_from_species.name);
                        }
                    });
                }
                else{
                    this.Evolution0.push(this.$route.params.pokeName);
                }
                URL = this.PokeEvoChain.evolution_chain.url;
                axios.get(URL).then(res3 =>{
                this.PokeEvo1 = res3.data.chain.evolves_to;
                if(this.PokeEvo1.length > 0){
                    this.PokeEvo1.forEach(element => {
                        this.Evolution1.push(element.species.name);
                        this.PokeEvo2 = element.evolves_to;
                    });
                    if(this.PokeEvo2.length > 0){
                        this.PokeEvo2.forEach(element2 => {
                        this.Evolution2.push(element2.species.name);
                        });
                    }
                }
                this.Evolution0.forEach(element => {
                    axios.get("https://pokeapi.co/api/v2/pokemon/"+element).then(r =>{
                        this.Img0.push(r.data.sprites.front_default);
                    });
                });
                this.Evolution1.forEach(element => {
                    axios.get("https://pokeapi.co/api/v2/pokemon/"+element).then(r =>{
                        this.Img1.push(r.data.sprites.front_default);
                    });
                });
                this.Evolution2.forEach(element => {
                    axios.get("https://pokeapi.co/api/v2/pokemon/"+element).then(r =>{
                        this.Img2.push(r.data.sprites.front_default);
                    });
                });
                });
            });
        });
    },
    updated: function(){
        axios.get("https://pokeapi.co/api/v2/pokemon/"+this.$route.params.pokeName).then(res =>{
            this.PokeMoves = res.data.moves;
            this.PokeCarac.Peso = res.data.weight;
            this.PokeCarac.Altura = res.data.height;
        });
    },
    data(){
        return{
            PokeMoves: [],
            PokeCarac: {
                Altura: '',
                Peso: '',
            },
            Img0: [],
            Img1: [],
            Img2: [],
            PokeEvo0: [],
            PokeEvo1: [],
            PokeEvo2: [],
            Evolution0: [],
            Evolution1: [],
            Evolution2: [],
            PokeEvoChain: []
        }
    },
    filters:{
        upper: function(value){
            var newName = value[0].toUpperCase() + value.slice(1);
            return newName;
        }
    }
}
</script>

<style>
    .center-textt{
        text-align: center;
    }
    .b-1px{
        display: inline-block;
        width: 100%;
    }
</style>