<template>
    <div>
        
    <div class="column is-half is-offset-one-quarter">
        <router-link :to="{name: 'Pokedex'}"><button class="button is-info is-rounded">Casa</button></router-link>
        <table class="table is-fullwidth">
            <thead>
                <tr>
                <th><abbr title="Number">Number</abbr></th>
                <th><abbr title="Move">Move</abbr></th>
                </tr>
            </thead>
            <tfoot>
                <tr>
                <th><abbr title="Number">Number</abbr></th>
                <th><abbr title="Move">Move</abbr></th>
                </tr>
            </tfoot>
            <tbody>
                <tr v-for="(poke,index) in PokeMoves" :key="index">
                    <td>{{index}}</td>
                    <td>{{poke.move.name}}</td>
                </tr>
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
            //console.log(res.data.moves);
            this.PokeMoves = res.data.moves;
            console.log(this.PokeMoves);
        });
    },
    data(){
        return{
            PokeMoves : []
        }
    }
}
</script>

<style>

</style>