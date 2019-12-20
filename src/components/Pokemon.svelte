<script>
  import { onMount } from "svelte";

  let name = [];
  let description = [];
  let number = getRandomNumber(1, 809);
  let type = [];

  function getRandomNumber(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min + 1)) + min;
  }

  $: if (number <= 9) {
    number = "00" + number;
  } else if (number <= 99) {
    number = "0" + number;
  }

  async function findPokemon() {
    const url = "https://pokeapi.co/api/v2/pokemon-species/" + number;
    const response = await fetch(url);
    const data = await response.json();
    name = data.names[2].name;
    const language = data.flavor_text_entries[1].language.name;
    const allLanguages = data.flavor_text_entries;
    const filtered = allLanguages.filter(lang => lang.language.name === "en");
    description = filtered[0].flavor_text;
  }

  // async function findPokemonName() {
  //   const urlName = "https://pokeapi.co/api/v2/pokemon/1";
  //   const responseName = await fetch(urlName);
  //   data = await responseName.json();
  //   name2 = data.name;
  //   console.log(name2);
  //   url2 = data.species.url;
  //   console.log(url2);
  //   type = data.types[0].type.name;
  //   console.log(type);
  // }

  function handleClick() {
    number = getRandomNumber(1, 809);
    findPokemon();
  }

  onMount(() => {
    findPokemon();
  });
</script>

<style>
  img {
    height: 250px;
  }
  button {
    background: lightgoldenrodyellow;
    border: 1px solid black;
    font-size: 24px;
    padding: 8px 16px;
  }
</style>

{#await name}
  <p>... waiting</p>
{:then name}
  <h2>{name}</h2>
  <img
    src="http://assets.pokemon.com/assets/cms2/img/pokedex/full/{number}.png"
    alt="pokemon" />
  <p>{description}</p>
{/await}

<button on:click={handleClick}>Find another pokemon</button>
