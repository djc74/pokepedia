<script>
  import { onMount } from "svelte";

  let data = [];
  let description = [];
  let name = [];
  let number = getRandomInteger(100, 808);

  function getRandomInteger(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min + 1)) + min;
  }

  function newNumber() {
    number = getRandomInteger(100, 808);
  }

  function handleClick() {
    newNumber();
    console.log(number);
  }

  onMount(async () => {
    const result = await fetch(
      "https://pokeapi.co/api/v2/pokemon-species/" + number
    );
    data = await result.json();
    description = data.flavor_text_entries[1].flavor_text;
    name = data.names[2].name;
  });
</script>

<style>
  img {
    height: 250px;
  }
  button {
    border: 1px solid black;
    font-size: 24px;
    padding: 8px 16px;
  }
</style>

<div>
  <h2>{name}</h2>
  <img
    src="http://assets.pokemon.com/assets/cms2/img/pokedex/full/{number}.png"
    alt="pokemon" />
  <p>{description}</p>
  <button on:click={handleClick}>Find a pokemon</button>
</div>
