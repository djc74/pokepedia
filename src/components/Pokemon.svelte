<script>
  import { onMount } from "svelte";

  let data = [];
  let description = [];
  let name = [];

  onMount(async () => {
    const result = await fetch(
      "https://pokeapi.co/api/v2/pokemon-species/132/"
    );
    data = await result.json();
    description = data.flavor_text_entries[2].flavor_text;
    name = data.names[2].name;
  });
</script>

<style>
  img {
    height: 250px;
  }
</style>

<div>
  <img
    src="http://assets.pokemon.com/assets/cms2/img/pokedex/full/132.png"
    alt="pokemon" />
  <h2>{name}</h2>
  <p>{description}</p>
  <p>{number}</p>
</div>
