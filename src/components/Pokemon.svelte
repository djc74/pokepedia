<script>
  import { onMount } from "svelte";

  let data = [];
  let description = [];
  let name = [];
  let number = getRandomNumber(100, 808);

  function getRandomNumber(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min + 1)) + min;
  }

  async function findPokemon() {
    const url = "https://pokeapi.co/api/v2/pokemon-species/" + number;
    const response = await fetch(url);
    data = await response.json();
    description = data.flavor_text_entries[1].flavor_text;
    name = data.names[2].name;
  }

  onMount(() => {
    findPokemon();
  });

  function handleClick() {
    number = getRandomNumber(100, 808);
    findPokemon();
  }
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
