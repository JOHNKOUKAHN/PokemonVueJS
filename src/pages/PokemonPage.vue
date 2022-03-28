<template>
  <div>
    <h1 v-if="!pokemon">Cargando...</h1>
     <div v-else>
      <h1>Quien es este pokemon?</h1>
      <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
      <PokemonOptions :pokemons="pokemonArr" @selection="checkAnswer" />
      <template v-if="showAnswer">
      <h2>{{message}}</h2>
      <button @click="newGame">Reiniciar</button>
      </template>
    </div>
  </div>
</template>

<script>
import PokemonPicture from '@/components/PokemonPicture'
import PokemonOptions from '@/components/PokemonOptions'
import getPokemonOptions from '@/helpers/getPokemonOptions'

getPokemonOptions()
export default {
  components:{
    PokemonPicture,
    PokemonOptions
  },
  data(){
    return{
      pokemonArr: [],
      pokemon: null,
      showPokemon : false,
      showAnswer: false,
      message: null      
    }
  },
  methods:{
    async mixPokemonArray(){
      this.pokemonArr = await getPokemonOptions()
      console.log(this.pokemonArr)
      
    const rndInt = Math.floor(Math.random() * 4)
    this.pokemon = this.pokemonArr[rndInt]
    },
    checkAnswer(pokemonId){
      this.showPokemon = true

      if (pokemonId === this.pokemon.id) {
        this.message = `Correcto ${this.pokemon.name}`
      }
      else{
        this.message = `Oops.. era:  ${this.pokemon.name}`
      }

      this.showAnswer = true
    },

    newGame(){
       this.pokemonArr = []
      this.pokemon = null
      this.showPokemon = false
      this.showAnswer= false
      this.message= null 
      
    this.mixPokemonArray()
    }
  },
  mounted(){
    this.mixPokemonArray()

  }

}
</script>
