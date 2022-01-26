<script context="module">
    export async function load({ fetch, params }) {
        const url = `https://pokeapi.co/api/v2/pokemon/?limit=${params.num}`;
        const res = await fetch(url);
        const data = await res.json();
        const loadedPokemon = data.results.map((data, index) => {
            return {
                name: data.name,
                id: index + 1,
                image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${
                    index + 1
                }.png`
            };
        });
        return {props: { pokemon:loadedPokemon }}
    }
</script>
<script>
    //import { pokemon } from "../stores/pokestore";
    import PokemanCard from "../components/pokemanCard.svelte";

    export let pokemon;

    let searchTerm = "";
    let filteredPokemon = []; 

    $: {
        console.log(searchTerm);
        if(searchTerm) {
            filteredPokemon = pokemon.filter(pokeman => pokeman.name.toLowerCase().includes(searchTerm.toLowerCase()));
        }
        else {
            filteredPokemon = [...pokemon]
        }
    }
</script>

<svelte:head>
    <title>Svelte Kit pokedex</title>
</svelte:head>

<h1 class="text-2xl text-center my-8 uppercase">Welcome to Svelte Kit Pokedex</h1>

<input class="w-full rounded-md text-lg p-4 border-2 border-gray-200" type="text" bind:value={searchTerm} placeholder="Search Pokemon">

<div class="py-4 grid gap-4 md:grid-cols-2 grid-cols-1">
    {#each filteredPokemon as pokeman}
        <PokemanCard pokeman={pokeman}/>
    {/each}
</div>
