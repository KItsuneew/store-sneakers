<script setup>
import axios from 'axios';
import Popular from '../components/Popular.vue';
import CardList from '../components/CardList.vue';
import { inject, reactive, watch, ref, onMounted } from 'vue';

const { addToCart, removeToCart, cartItems } = inject('cartActions');

const items = ref([]);

const filters = reactive({
	sortBy: 'title',
	searchQuery: '',
});

const addToFavorite = async (item) => {
	try {
		if (!item.isFavorite) {
			const obj = {
				item_id: item.id,
			};

			const { data } = await axios.post(
				`https://1902939eeee538b4.mokky.dev/favorites`,
				obj
			);
			item.isFavorite = true;
			item.favoriteId = data.id;
		} else {
			item.isFavorite = false;
			await axios.delete(
				`https://1902939eeee538b4.mokky.dev/favorites/${item.favoriteId} `
			);
			item.favoriteId = null;
		}
	} catch (err) {
		console.log(err);
	}
};

const onClickAddToPlus = (item) => {
	if (!item.isAdd) {
		addToCart(item);
	} else {
		removeToCart(item);
	}
};

const onChangeSearchInput = (event) => {
	filters.searchQuery = event.target.value;
};

const onChangeSelect = (event) => {
	filters.sortBy = event.target.value;
};

const fetchItems = async () => {
	try {
		const params = {
			sortBy: filters.sortBy,
		};

		if (filters.searchQuery) {
			params.title = `*${filters.searchQuery}*`;
		}

		const { data } = await axios.get(
			'https://1902939eeee538b4.mokky.dev/items',
			{
				params,
			}
		);

		items.value = data.map((obj) => ({
			...obj,
			isFavorite: false,
			favoriteId: null,
			isAdd: false,
		}));
	} catch (err) {
		console.log(err);
	}
};

const fetchFavorites = async () => {
	try {
		const { data: favorites } = await axios.get(
			'https://1902939eeee538b4.mokky.dev/favorites'
		);

		items.value = items.value.map((item) => {
			const favorite = favorites.find(
				(favorite) => favorite.item_id === item.id
			);

			if (!favorite) {
				return item;
			}

			return {
				...item,
				isFavorite: true,
				favoriteId: favorite.id,
			};
		});
	} catch (err) {
		console.log(err);
	}
};

onMounted(async () => {
	const localCart = localStorage.getItem('cartItems');
	cartItems.value = localCart ? JSON.parse(localCart) : [];

	await fetchItems();
	await fetchFavorites();

	items.value = items.value.map((item) => ({
		...item,
		isAdd: cartItems.value.some((cartItem) => cartItem.id === item.id),
	}));
});

watch(filters, fetchItems);

watch(cartItems, () => {
	items.value = items.value.map((item) => ({
		...item,
		isAdd: false,
	}));
});
</script>

<template>
	<Popular />
	<div>
		<div class="flex justify-between items-center">
			<h2 class="font-bold text-3xl pb-10">Все кроссовки</h2>

			<div class="flex gap-4">
				<select
					@change="onChangeSelect"
					class="py-2 px-3 border rounded-md outline-none"
				>
					<option value="name">По названию</option>
					<option value="price">По цене (дешевые)</option>
					<option value="-price">По цене (дорогие)</option>
				</select>

				<div class="relative">
					<img class="absolute left-4 top-3" src="/search.svg" alt="arrow" />
					<input
						@input="onChangeSearchInput"
						class="border rounded-xl py-2 pl-11 pr-4 outline-none focus:border-gray-400"
						placeholder="Поиск..."
						type="text"
					/>
				</div>
			</div>
		</div>

		<div class="mt-5">
			<CardList
				:items="items"
				@add-to-favorite="addToFavorite"
				@add-to-cart="onClickAddToPlus"
			/>
		</div>
	</div>
</template>
