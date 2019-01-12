<template>
  <div id="app">
    <h1 class="threed">Rick and morty stickers</h1>
    <section v-if="errored">
      <p class="error">We're sorry, we're not able to retrieve this information at the moment,
        please try back later</p>
    </section>
    <section v-else>
      <div v-if="loading">
        <Spinner />
      </div>
      <div v-else class="container">
        <Sticker
          v-for="character in characters"
          :key="character.id"
          :character="character"
        ></Sticker>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios';
import Sticker from '@/components/Sticker.vue';
import Spinner from '@/components/Spinner.vue';

export default {
  name: 'app',
  components: {
    Spinner,
    Sticker,
  },
  data() {
    return {
      characters: null,
      loading: true,
      errored: false,
    };
  },
  mounted() {
    let counter = 5;
    while (counter > 0) {
      axios
        .get('https://rickandmortyapi.com/api/character',
          { params: { page: counter } })
        .then((response) => {
          (this.characters === null)
            ? this.characters = response.data.results
            : this.characters = this.characters.concat(response.data.results);
        })
        .catch((error) => {
          this.errored = true;
          console.log(error);
        })
        .finally(() => {
          this.loading = false;
        });

      counter -= 1;
    }
  },
};
</script>

<style lang="scss">
  @import url("https://fonts.googleapis.com/css?family=Dancing+Script:700");

  body {
    margin: 0 auto;
    max-width: 56em;
    padding: 1em 0;
  }

  html {
    -webkit-font-smoothing: antialiased;
  }

  .container {
    display: flex;
    flex-wrap: wrap;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(5rem, 1fr));
    grid-gap: 1.4em;
  }

  @supports (display: grid) {
    .module {
      margin: 0;
    }
  }

  .threed {
    font-size: 5rem;
    font-family: "Dancing Script";
    text-align: center;
    font-weight: 700;
    color: white;
    -webkit-text-stroke: 1px rgba(0, 0, 0, 0.2);
    text-shadow: -10px 0 rgba(0, 255, 255, 0.4), 10px 0 rgba(255, 0, 255, 0.4);
  }

  img {
    width: 5rem;
    height: 5rem;
    border-radius: 0.5rem;
    box-shadow: 0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23);
  }

  .error {
    text-align: center;
    color: #c6c6c6;
  }

  .grid-item:hover {
    transition: transform 500ms;
    transform: scale(1.1);
  }

  .bounce { animation: bounce 1s; }

  @keyframes bounce {
    0% { transform: rotate(  0deg) scale(1.0); }
    33% { transform: rotate( 12deg) scale(1.2); }
    67% { transform: rotate(-12deg) scale(0.8); }
    100% { transform: rotate(  0deg) scale(1.0); }
  }
</style>
