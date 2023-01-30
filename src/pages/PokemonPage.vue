<template>
    <h1 v-if="!pokemon">Espere por favor...</h1>
    <div v-else>
        <h1>¿Quien es este pokémon?</h1>


        <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
        <PokemonOptions :gameOver="gameOver" :pokemons="pokemonArr" @selection="checkAnswer($event)"/>
        <PokemonLifes :lifes="lifes"/>

        <template v-if="showAnswer">
            <h2 class="fade-in">{{ message }}</h2>
            <button @click="newGame">Nuevo Juego</button>
        </template>

    </div>
</template>

<script>
import PokemonLifes from '@/components/PokemonLifes.vue';
import PokemonOptions from '@/components/PokemonOptions.vue';
import PokemonPicture from '@/components/PokemonPicture.vue';

import getPokemonOptions from '@/helpers/getPokemonOptions.js';

const sound = require("@/assets/sounds/sound.mp3");

export default {
    components: { PokemonPicture, PokemonOptions, PokemonLifes },
    data(){
        return{
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: '',
            sound,
            lifes: 2,
            gameOver: false
        }
    },
    methods:{
        async mixPokemonArray(){
            this.pokemonArr = await getPokemonOptions()

            const randInt = Math.floor(Math.random() * 4)
            this.pokemon = this.pokemonArr[randInt]
        },
        checkAnswer(selectedId){
            this.showPokemon = true;
            this.showAnswer  = true;


            if(this.pokemon.id != selectedId){
                this.message = `Incorrecto, el pokemon es ${this.pokemon.name}`;
                if(this.lifes <= 1){
                    this.lifes = 0;
                    this.gameOver = true;
                }else{
                    this.lifes--;
                }
            }else{
                this.message = `Correcto, el pokemon es ${this.pokemon.name}`;
            }
        },
        newGame(){
            this.pokemon = false,
            this.message = '',
            this.showPokemon = false,
            this.showAnswer = false,
            this.lifes = 2,
            this.gameOver = false,
            this.mixPokemonArray(),
            this.playSound()
        },
        playSound(){
            let beat = new Audio(this.sound);
            beat.addEventListener("canplaythrough", () => {
                beat.play();
            });

        }
    },
    mounted() {
        this.mixPokemonArray(),
        this.playSound()
    }
}
</script>

<style>

</style>