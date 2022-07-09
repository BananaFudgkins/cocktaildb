<script>
	import { onMount } from "svelte";

	import "carbon-components-svelte/css/g90.css";
	import {
		Header,
		Content,
		ImageLoader,
		Grid,
		Row,
		Column,
		DataTable
	} from "carbon-components-svelte";

	let promise = [];

	async function getDrinks() {
		let res = await fetch(
			"https://www.thecocktaildb.com/api/json/v1/1/random.php"
		);
		let data = await res.json();

		return data.drinks;
	}

	onMount(async () => {
		promise = await getDrinks();
	});
</script>

<Header company="Deja Jackson" platformName="Drink Recipes" />

<Content>
	<Grid padding>
		<Column>
			{#await promise}
				<p>Loading...</p>
			{:then drinks}
				{#each drinks as drink}
					<Row><ImageLoader src={drink.strDrinkThumb}/></Row>
					<Row><h1>{drink.strDrink}</h1></Row>

					<Row><h2>{drink.strCategory}</h2></Row>

					<Row><h2>{drink.strAlcoholic}</h2></Row>

					<Row>
						<DataTable
							headers = {[
								{ key: "name", value: "Ingredient Name"},
								{ key: "measure", value: "Measure"}
							]}
							rows = {[
								{
									id: "1",
									name: drink.strIngredient1,
									measure: drink.strMeasure1
								},
								{
									id: "2",
									name: drink.strIngredient2,
									measure: drink.strMeasure2
								},
								{
									id: "3",
									name: drink.strIngredient3,
									measure: drink.strMeasure3
								},
								{
									id: "4",
									name: drink.strIngredient4,
									measure: drink.strMeasure4
								},
							]}
						/>
					</Row>
					
					<Row><p>{drink.strInstructions}</p></Row>
				{/each}
			{/await}
		</Column>
	</Grid>
</Content>