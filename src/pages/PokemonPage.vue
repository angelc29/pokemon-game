<template>
    <h1 v-if="!pokemon">Espere un momento, por favor...</h1>

    <div v-else>
        <h1>¿Quíen es este pokemón?</h1>

        <PokemonPicture 
            :pokemonId='pokemon.id' 
            :showPoke='showPokemon'
        />
        <PokemonOptions 
            :pokemons='pokemonArr' 
            @selection-pokemon='checkAnswer'
        />

        <template v-if="showAnswer" class="fade-in">
            <h2>{{message}}</h2>
            <button @click='newGame'>
                Nuevo Juego
            </button>
        </template>

    </div>
</template>

<script>

import PokemonPicture from '../components/PokemonPicture'
import PokemonOptions from '../components/PokemonOptions'

import getPokemonOptions from '@/helpers/getPokemonOptions.js'



export default {
    name: 'PokemonPage',
    components: {
        PokemonPicture,
        PokemonOptions
    },
    data () {
        return {
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: ''
        }
    },
    methods: {
        async mixPokemonArray() {
            this.pokemonArr = await getPokemonOptions()
            const rndInt = Math.floor(Math.random()*4)
            this.pokemon = this.pokemonArr[rndInt]
        },
        checkAnswer(selectedPoke) {
            this.showPokemon = true
            this.showAnswer = true

            if(selectedPoke === this.pokemon.id){
                this.message = `Correcto! Es un ${this.pokemon.name}`
            } else {
                this.message = `Ups! Era un ${this.pokemon.name}`
            }
        },
        newGame(){
            this.pokemonArr = []
            this.pokemon = null
            this.showPokemon = false
            this.showAnswer = false
            this.mixPokemonArray()
        }
    },
    mounted(){
        this.mixPokemonArray()
    }
}
</script>