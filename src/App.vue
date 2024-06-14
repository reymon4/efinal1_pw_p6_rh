<template>
  <div class="container">
    <div class="juego" v-if="juego">
      <div class="cabecera">
        <h2>Puntos: {{ point }}</h2>
        <h2>Intentos: {{ intent }}</h2>
      </div>
      <div class="imagen_container">
        <Imagen
          v-for="(image, indice) in imagesUrl"
          :key="indice"
          :url="image"
          :text="names[indice]"
        />
      </div>
      <div class="botonJugar">
        <button @click="getAnswer">JUGAR</button>
      </div>
    </div>
    <div v-else>
      <div
        class="loser"
        v-if="gameOver && point < 10 && intent === 5"
        style="color: red"
      >
        <h1>Has utilizado tus cinco intentos</h1>
        <h2>El juego ha terminado, inténtalo nuevamente</h2>
        <button id="newGame" @click="newGame">Juego Nuevo</button>
      </div>
      <div v-if="gameOver && point >= 10" style="color: blue">
        <h1>Puntaje: {{ point }}</h1>
        <h2>Felicitaciones has ganado un premio de $10.000,00</h2>
        <button id="newGame" @click="newGame">Juego Nuevo</button>
      </div>
    </div>
  </div>
</template>

<script>
import Imagen from "@/components/Imagen.vue";
export default {
  name: "App",
  components: {
    Imagen,
  },
  data() {
    return {
      imagesUrl: [
        "https://via.placeholder.com/250",
        "https://via.placeholder.com/250",
        "https://via.placeholder.com/250",
      ],
      names: [],
      point: 0,
      intent: 0,
      juego: true,
      gameOver: false,
    };
  },
  methods: {
    async getAnswer() {
      this.intent++;
      const pokeImages = [];
      const pokeNames = [];
      const pokeId = [8, 56, 89, 15, 22];
      //NOMBRES E IMÁGENES TIENE LA MISMA LONGITUD
      for (let i = 0; i < this.imagesUrl.length; i++) {
        let number = this.generateRandom(pokeId);
        const { name } = await fetch(
          `https://pokeapi.co/api/v2/pokemon/${number}`
        ).then((r) => r.json());
        console.log(name);
        pokeNames.push(name);
        pokeImages.push(
          `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${number}.svg`
        );
      }
      this.names = pokeNames;
      this.imagesUrl = pokeImages;

      let aux = 0;

      if (this.names.length >= 3) {
        if (
          this.names[0] === this.names[1] &&
          this.names[0] === this.names[2]
        ) {
          aux = 3;
        } else if (
          this.names[0] === this.names[1] ||
          this.names[0] === this.names[2] ||
          this.names[1] === this.names[2]
        ) {
          aux = 2;
        }
      }

      if (aux === 3) {
        this.point = 5 + this.point;
      } else if (aux === 2) {
        this.point = this.point + aux;
      }

      if (this.intent >= 5 || this.point >= 10) {
        this.juego = false;
        this.gameOver = true;
      }
    },
    generateRandom(vector) {
      let randomIndex = Math.floor(Math.random() * vector.length);
      return vector[randomIndex];
    },
    newGame() {
      this.imagesUrl = [
        "https://via.placeholder.com/250",
        "https://via.placeholder.com/250",
        "https://via.placeholder.com/250",
      ];
      this.names = [];
      this.juego = true;
      this.point = 0;
      this.intent = 0;
    },
  },
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
}

.imagen_container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  align-content: center;
  justify-items: center;
}
.cabecera {
  display: flex;
  justify-content: space-around;
}
button {
  font-size: 25px;
}
.botonJugar {
  margin-top: 20px;
}
</style>
