<script>
	import Footer from "./Footer.svelte"
  import Nappi from ".//Nappi.svelte";

//Kaikki RestAPI:sta tulevat tiedot oli laitettava erikseen eri muuttujiin, koska en osannut survoa niitä hienosti yhtenä pakettina edes googlen avulla.
	let pokemonNumber;
	let endpoint = 'https://pokeapi.co/api/v2/pokemon/'
	let pokeNimi;
	let pokeHeight = "Please wait..."; //tämä ns. default arvo sille varalle, kun lataus API:sta kestää pidempään, niin ei näy "undefined" sillä aikaa.
	let pokeImage = "Please wait...";
	let pokeWeight = "Please wait...";
	let pokeNimiIsolla = "Please wait...";
	function validoiNumero(x) {
    return (x > 1010 || x < 1 || x == null);
  } //validoi numerolla haun, olemassa olevista Pokemoneista.

let haku = false //väli muuttuja, kunnes ensimmäinen haku haitettu.
	


let promise;
const pokemonSearch = (async () => {
  promise = fetch((endpoint)+pokemonNumber)
  .then(response => response.json())
  .then(data => {
		console.log(data);
		pokeNimi = data.name;	
		pokeHeight = data.height
		pokeWeight = data.weight
		pokeImage = data.sprites.front_default
		pokeNimiIsolla = pokeNimi.toUpperCase()

  }).catch(error => {
    console.log(error);
  });
	haku = true
} //Käytin googlea, ja en saanut melkein yhtäkään fetchiä toimimaan oikein, paitsi tämmöistä outoa viritelmää, jota jatko jalostin. Muistaakseni käytin tätä: https://sveltesociety.dev/recipes/component-recipes/using-fetch-to-consume-apis
);
</script>
<main>
<h1><img id="pokepallo" src="./pics/PokePallo.png" alt="PokéBall" height="150" width="150" />
	PokéDex
</h1>
<body>
	<p>Search by number: <input type="number" bind:value={pokemonNumber} min="1" max="1010"> <Nappi on:click={pokemonSearch} disabled={validoiNumero(pokemonNumber)}>Search</Nappi></p>
		{#if validoiNumero(pokemonNumber)} 
		<p>Please insert a number between 1-1010!</p>
		{/if}<!--Numerolla etsintä, joka syöttää annetun numeron fetchiin, jolla se hakee tietyn objectin. Validointi tehty helposti, että se on numero, ja että on oikea arvoltaan. -->
	<p>
{#if haku}
<div class="container">
		<div class="image">
<img id="pokemonImage" src="{pokeImage}" alt="{pokeNimi}" height="750" width="750">
		</div>
	<div class="text">
		<p>Name: {pokeNimiIsolla}</p>
		<p>Height: {pokeHeight}</p>
		<p>Weight: {pokeWeight}</p>
	</div>
</div><!--Erittäin huono yritys saada, tiedot hienosti näkyville, mutta kaikkea tietoa en saanut API:sta ulos. -->
{/if}
</p>

</body>
<Footer/>
</main>

<style>

body {
	background-color: #ee1515;
	font-weight: bold
}

	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}
	#pokepallo{

padding: 0.2em;
	}
	.container {
  display: flex;
  align-items: center;
  justify-content: center
}

img {
  max-width: 100%
}

.image {
  flex-basis: 40%
}

.text {
	border: 2px solid black;
		font-size: 6em;
	text-align: left;
  
  padding: 0.3em;
}

#pokemonImage{
	position: top left
}
	h1 {
		background-color: #ee1515;
		font-family: fantasy;
		color: #222224;
		text-transform: uppercase;
		font-size: 8em;
		font-weight: 100;
		margin-top: -0.2em;
		border-radius: 0.25em;
	}
	body {
		background-color: rgb(221, 230, 230);
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>