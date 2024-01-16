import PokemonPicture from '/components/PokemonPicture';
<template>
   <h1 v-if="!pokemon"> Espere por favor...</h1>
   <div v-else>
      <h1>¿Quién es este pokémon?</h1>
      <PokemonPicture  :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
      <PokemonOptions  :pokemons="pokemonArr" @selection="checkAnswer( $event )"/>
      <div v-if="showPokemon" class="content">         
         <h2 :class="`fade-in ${sucess == true ? 'green' : 'red'}`">{{ message }}</h2>
         <button @click="newGame">
              Nuevo Juego
         </button>
      </div>
   </div>
</template>
<script>
  import PokemonPicture from '@/components/PokemonPicture'
  import PokemonOptions from '@/components/PokemonOptions';
  import getPokemonOptions from '@/helpers/getPokemonOptions'

export default {
    name: 'PokemonPage',
    components: {
      PokemonPicture,
      PokemonOptions
    },
    data() {
        return{

           pokemonArr: [],
           pokemon: null,
           showPokemon: false,
           showAnswer: false,
           message: '',
           sucess: false
        }
    },
    methods:{
       async mixPokemonArray(){
          this.pokemonArr = await getPokemonOptions()

          const rndInt = Math.floor( Math.random() * 4)
          this.pokemon = this.pokemonArr[ rndInt ]
       },
       checkAnswer(selectedId){
            this.showPokemon = true
            if( selectedId === this.pokemon.id){
               this.message = `Correcto, es ${ this.pokemon.name }`
               this.sucess = true
            }else{
               this.message = `Oops, era ${ this.pokemon.name }`
               this.sucess = false
            }
       },
       newGame(){

         this.showPokemon = false
         this.showAnswer = false
         this.sucess = false
         this.pokemon = null
         this.pokemonArr = []
         this.mixPokemonArray()

       }
    },
    mounted(){
       this.mixPokemonArray()
    }
  }
  </script>
  <style scoped>
   .content{
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
   }

   .green{
      color: green;
      font-weight: bold;
   }
   
   .red{
      color: #CB3234;
      font-weight: bold;
   }

   h2{
      margin-top: 0;
      padding-top: 0;
   }

  </style>