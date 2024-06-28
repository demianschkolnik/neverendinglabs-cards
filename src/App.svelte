<script>
	import { onMount } from "svelte";
	import CardList from "./Cards.svelte";
	import Card from "./lib/components/CardProxy.svelte";
  
	let basics = [];
	let isLoading = true;
  
	const getCards = async () => {
	  let cardFetch = await fetch("/data/cards.json");
	  let cards = await cardFetch.json();
	  return cards;
	};
  
	const loadCards = async() => {
	  return getCards()
		.then((cards) => {
		  basics = cards.slice(1, 7); // Assuming common cards are in this range
		  isLoading = false;
		});
	};
  
	onMount(() => {
	  loadCards();
	});
  </script>
  
  <main>
	<header>
	  <h1>Neverearth Cards</h1>
	</header>
  
	{#if isLoading}
	  <p>Loading...</p>
	{:else}
	  <CardList>
		{#each basics as card}
		  <Card
			id={card.id}
			name={card.name}
			img={card.images.large}
			number={card.number}
			types={card.types}
			supertype={card.supertype}
			subtypes={card.subtypes}
		  />
		{/each}
	  </CardList>
	{/if}
  </main>
  
  <style>
	main {
	  padding: 1em;
	  font-family: Arial, sans-serif;
	}
  
	header {
	  text-align: center;
	  margin-bottom: 2em;
	}
  </style>
  