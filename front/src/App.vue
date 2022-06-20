<template>
	<h1>Shop Stock</h1>
	<button v-on:click="get_stock">Update</button>
	<h2>Total Worth: £{{total_worth}}</h2>
	<div v-for="stock_item in stock">
		<h2>{{stock_item.name}}</h2>
		<h3 class="inline">Amount: {{stock_item.amount}}</h3>
		<button v-on:click="edit_amount(stock_item.name, 1)">Add</button>
		<button v-on:click="edit_amount(stock_item.name, -1)">Remove</button>
		<h3>Price: £{{stock_item.price}}</h3>
	</div>
</template>

<script setup>
import { ref } from "vue";

let stock = ref([]);
let total_worth = ref(0);

const get_stock = () => {
	fetch("http://localhost:5000/stock", {
		headers: {
			"Content-Type": "application/json"
		}
	}).then(response => response.json()).then(data => {
		stock.value = data;

		// Calculates the new total stock worth every update
		total_worth.value = 0;
		data.forEach(stock_item => total_worth.value += stock_item.amount * stock_item.price);
	});
};

// Allows users to add/remove stock
const edit_amount = (name, edit_amount) => {
	fetch("http://localhost:5000/edit-stock-amount", {
		method: "POST",
		headers: {
			"Content-Type": "application/json"
		},
		body: JSON.stringify({
			message: name,
			data: edit_amount
		})
	}).then(get_stock);
};

get_stock();
</script>

<style>
.inline {
	display: inline;
}
</style>