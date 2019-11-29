<script>
  import { onMount } from "svelte";

  let pokemon = [];

  onMount(async () => {
    const res = await fetch(`https://pokeapi.co/api/v2/pokemon/ditto`);
    pokemon = await res.json();
  });
</script>

<div>
  <h3>{pokemon.name}</h3>
</div>
