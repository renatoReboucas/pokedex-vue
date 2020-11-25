<template>
  <div id="app" class="container">
    <div class="columns is-mobile is-half">
      <h4 class="is-size-3 title">Pokedex</h4>
    </div>
    <hr>
    <div class="columns is-half is-offset-one-quarter is-mobile">
    <div class="control filter">
      <input 
      type="text" 
      class="input rounded-left" 
      placeholder="Buscar pokémon pelo nome"
      v-model="busca"
      v-on:keyup.enter="buscar()"
    >
    <button class="button is-success btn" @click="buscar()" >Buscar</button>
    </div>
    <br>
    </div>
    <div class="" v-for="(poke, index) in filteredPokemons" :key="poke.url">
      <!-- <div class="column is-half is-offset-one-quarter"> -->
        <div class="section-card">
        <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
        </div>
      <!-- </div> -->
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon.vue";

export default {
  components: { Pokemon },
  name: "App",
  comments: {
    Pokemon,
  },
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    };
  },
  created() {
    this.getPokemon();
  },
  methods: {
    getPokemon() {
      axios
        .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
        .then((res) => {
          // console.log(res.data.results);
          this.pokemons = res.data.results;
          this.filteredPokemons = res.data.results
        })
        .catch((error) => console.log("DEU RUIM", error));
    },
    buscar(){
      this.filteredPokemons = this.pokemons
      if( this.busca == '' || this.busca == ' ' ){
        this.filteredPokemons = this.pokemons
      }else{
        this.filteredPokemons = this.pokemons.filter( pokemon => pokemon.name == this.busca.toLowerCase() )
      }
    },
  },
  //usado pra fazer filtro, colocar funcao no v-for
  computed:{
    resultadoBusca(){
      if( this.busca == '' || this.busca == ' ' ){
        return this.pokemons
      }else{
        return this.pokemons.filter( pokemon => pokemon.name == this.busca.toLowerCase() )
      }
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-wrap: wrap;
  /* flex-direction: column; */
  justify-content: space-around;
  align-items: center;
}
.logo{
  border: 0px !important;
}
.filter{
  display: flex;
  justify-content: space-between;
}
.rounded-left{
  border-radius: 20px 0 0 20px !important;
}
.btn{
  border-radius: 0 4px 4px 0 !important;
}
/* .section-card{
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  align-items: center;
} */
@media(max-width: 500px){
  #app{
    /* flex-wrap: nowrap; */
  }
  .title{
  display: flex !important;
  justify-content: center !important;
  align-items: center !important;
}
}
</style>
