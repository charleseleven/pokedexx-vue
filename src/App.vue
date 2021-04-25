<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/pikachu.png" alt="Logo Pikachu" class="logo">
      <hr>
      <h4 class="is-size-1">Pokedexx</h4>
      <input type="text" class="input is-rounded" placeholder="Buscar pokemon pelo nome" v-model="busca" />
      <button class="button is-success" @click="buscar">Buscar</button>
      <div v-for="(pokemon, index) in filteredPokemons" :key="pokemon.url">
        <Pokemon :name="pokemon.name" :url="pokemon.url" :num="index + 1"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Pokemon from './components/Pokemon.vue'
export default {
  name: 'App',
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  created: function () {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(res => {
      console.log('Pegou a lista de pokemons')
      this.pokemons = res.data.results
      this.filteredPokemons = res.data.results
    })
  },
  components: {
    Pokemon
  },
  methods: {
    buscar: function () {
      this.filteredPokemons = this.pokemons
      if (this.busca === '' || this.busca === ' ') {
        this.filteredPokemons = this.pokemons
      } else {
        // this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name === this.busca)
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.match(this.busca.toLocaleLowerCase()))
      }
    }
  },
  computed: {
    // resultadoBusca: function () {
    //   if (this.busca === '' || this.busca === ' ') {
    //     return this.pokemons
    //   } else {
    //     return this.pokemons.filter(pokemon => pokemon.name === this.busca)
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
  color: #41b883;
}
body {
  margin: 0;
  background: #364253;
}
.logo {
  width: 300px;
  height: 360px;
}
.input {
  margin-bottom: 15px;
}
.button {
  margin-bottom: 25px;
}

</style>
