<template>
  <div id="pokemon" class="">
    <div class="card">
      <img class=" mx-auto d-block" :src="currentImg" alt="image pokemon" />
      <div class="card-body">
        <h5 class="card-title">#{{ num }} {{ name | upper }}</h5>
        <p class="subtitle is-6">{{ pokemon.type }}</p>
      </div>
      <div class="card-footer">
        <button
          type="button"
          class="btn btn-outline-dark"
          @click="mudarSprite()"
        >
          Mudar Sprite
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  props: {
    name: String,
    url: String,
    num: Number,
  },
  data() {
    return {
      isFront: true,
      currentImg: "",
      pokemon: {
        type: "",
        front: "",
        back: "",
      },
    };
  },
  filters: {
    upper(value) {
      let newName = value[0].toUpperCase() + value.slice(1);
      return newName;
    },
  },
  created: function() {
    axios.get(this.url).then((res) => {
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.currentImg = this.pokemon.front;
    });
  },
  methods: {
    mudarSprite() {
      if (this.isFront) {
        this.isFront = false;
        this.currentImg = this.pokemon.back;
      } else {
        this.isFront = true;
        this.currentImg = this.pokemon.front;
      }
    },
  },
};
</script>

<style scoped>
.space {
  margin: 0 0 15px 0;
}
#pokemon {
  margin-top: 2vw;
  /* margin-right: 2vh; */
}
.img-poke{
  width: 15%;
  height: 20%;
}
</style>
