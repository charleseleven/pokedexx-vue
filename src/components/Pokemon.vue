<template>
  <div class="card">
    <div class="card-image">
      <figure>
        <img :src="currentImg">
      </figure>
    </div>
    <div class="card-content">
      <div class="media">
        <div class="media-content">
          <p class="title is-4">{{ num }} - {{ upper }}</p>
          <p class="subtitle is-6">{{ pokemonData.type }}</p>
        </div>
      </div>

      <div class="content">
        <button class="button is-fullwidth" @click="mudarSprite">Mudar Sprite</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  created: function () {
    axios.get(this.url).then(res => {
      this.pokemonData.type = res.data.types[0].type.name
      this.pokemonData.front = res.data.sprites.front_default
      this.pokemonData.back = res.data.sprites.back_default
      this.currentImg = this.pokemonData.front
      console.log(this.pokemonData)
    })
  },
  data() {
    return {
      isFront: true,
      currentImg: '',
      pokemonData: {
        type: '',
        front: '',
        back: ''
      }
    }
  },
  props: {
    num: Number,
    name: String,
    url: String
  },
  computed: {
    upper() {
      return this.name[0].toUpperCase() + this.name.slice(1)
    }
  },
  methods: {
    mudarSprite: function () {
      if (this.isFront) {
        this.isFront = false
        this.currentImg = this.pokemonData.back 
      } else {
        this.isFront = true
        this.currentImg = this.pokemonData.front
      }
    }
  }
}
</script>

<style>
  .card {
    margin-bottom: 20px;
  }
  
</style>