<template>
    <div>

        <div class="card op">
            
        <div class="card-content">
            <div class="media">
            <div class="media-content">
                <p class="title is-4">{{pokemon.pokeid}} - {{name | upper}}</p>
                <div class="imagens">
                    <figure>
                    <img :src="currentfront" alt="Pokemon Frente">
                    </figure>
                    <figure>
                    <img v-if="currentback != null" :src="currentback" alt="Pokemon Costas">
                    </figure>
                    <!--
                    <figure>
                    <img :src="pokemon.frontshiny" alt="Pokemon Frente">
                    </figure>
                    <figure>
                    <img :src="pokemon.backshiny" alt="Pokemon Costas">
                    </figure>
                    -->
                </div>
                <img class="" :src="require(`../assets/${pokemon.type1}.gif`)">
                <img v-if="pokemon.type2 != ''" class="" style="margin-left:5px" :src="require(`../assets/${pokemon.type2}.gif`)">

            </div>
            </div>

            <div class="content">
                <button class="button is-info is-rounded" @click="mudarSprite">Trocar Sprite</button>
                
                <router-link :to="{name: 'InfoPokemon', params: {pokeName: name}}"><button class="button is-info is-rounded">Informações</button></router-link>
            </div>
        </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    created: function(){
        axios.get(this.url).then(res =>{
            //console.log(res.data);
            this.pokemon.type1 = res.data.types[0].type.name;
            if(res.data.types.length == 2)
                this.pokemon.type2 = res.data.types[1].type.name;
            this.pokemon.front = res.data.sprites.front_default;
            this.pokemon.frontshiny = res.data.sprites.front_shiny;
            this.pokemon.back = res.data.sprites.back_default;
            this.pokemon.backshiny = res.data.sprites.back_shiny;
            this.pokemon.pokeid = res.data.id;
            //console.log(this.pokemon);          
            this.currentfront = this.pokemon.front;
            this.currentback = this.pokemon.back;
        });
     
    },
    data(){
        return{
            pokemon: {
                type1: 'grass',
                type2: '',
                front: '',
                back: '',
                pokeid: ''
            },
            isShiny : false,
            currentfront: '',
            currentback: ''
        }
    },
    props:{
        num: Number,
        name: String,
        url: String
    },
    filters:{
        upper: function(value){
            var newName = value[0].toUpperCase() + value.slice(1);
            return newName;
        }
    },
    computed:{
        retornaUrlType(){
            return "../assets/water.png"; 
        }
    },
    methods:{
        mudarSprite: function(){
            if(this.isShiny){
            this.isShiny = false;
            this.currentfront = this.pokemon.front;
            this.currentback = this.pokemon.back;
            }
            else
            {
            this.isShiny = true;
            this.currentfront = this.pokemon.frontshiny;
            this.currentback = this.pokemon.backshiny;
            }
        }
    }
}
</script>

<style>
.type{
    width: 50px;
    height: 40px;
}
.op{
    border: 1px solid red;
}

.imagens{
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
}
</style>