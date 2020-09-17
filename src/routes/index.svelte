<script>
	import { onMount } from 'svelte';
  import { stores, goto } from "@sapper/app";
	const { page } = stores();
	let searchItems = ['Jim', 'Pam', 'Dwight', 'Michael', 'Andy', 'Ryan', 'Kelly', 'Darryl', 'Kevin', 'Phyllis', 'New Jim', 'New Dwight', 'Creed']
	let items = searchItems
	let term;
  let query = $page.query.term; // Replace term with your param
  let path = $page.path;
  
  function searchTerm() {
    items = searchItems.filter(item => item.includes(term))
  }

  function search() {
    // This would be the function you call when someone "searches"
    goto(`${path}?term=${term}`)
    searchTerm()
  }

  onMount(()=> {
    // Check if the query string term is set, set term to equal it and search
    if (typeof query !== "undefined") {
     term = decodeURI(query)
     searchTerm()
    }
  })
</script>

<h1>Search for stuff</h1>

<input type="text" bind:value={term}>
<button on:click={search}>search 🔍</button>

{#each items as item}
 <div>
	 <p>{item}</p>
 </div>
 <hr>
{:else}
<p>Sorry, that's not an employee at Dunder Mifflin</p>
<small>*This directory may not be up to date</small>
{/each}