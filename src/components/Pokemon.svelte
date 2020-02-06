<script>
  import { onMount } from "svelte";

  let name = [];
  let description = [];
  let number = getRandomNumber(1, 809);

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
    const allLanguages = data.flavor_text_entries;
    const filteredForEnglish = allLanguages.filter(
      lang => lang.language.name === "en"
    );
    description = filteredForEnglish[0].flavor_text;
    console.log(number);
  }

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
