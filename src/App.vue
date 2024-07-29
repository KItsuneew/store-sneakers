<script setup>
import Header from './components/Header.vue';
import Drawer from './components/Drawer/Drawer.vue';
import { onMounted, reactive, ref, watch, provide, computed } from 'vue';
import axios from 'axios';
import CartItem from './components/Drawer/CartItem.vue';

const cartItems = ref([]);

const totalPrice = computed(() =>
	cartItems.value.reduce((acc, item) => acc + item.price, 0)
);

const priceToTax = computed(() => Math.round(totalPrice.value * 0.05));

const drawerOpen = ref(false);

const closeDrawer = () => {
	drawerOpen.value = false;
};

const openDrawer = () => {
	drawerOpen.value = true;
};

const addToCart = (item) => {
	cartItems.value.push(item);
	item.isAdd = true;
};

const removeToCart = (item) => {
	cartItems.value.splice(cartItems.value.indexOf(item), 1);
	item.isAdd = false;
};

watch(
	cartItems,
	() => {
		localStorage.setItem('cartItems', JSON.stringify(cartItems.value));
	},
	{ deep: true }
);

provide('cartActions', {
	cartItems,
	closeDrawer,
	openDrawer,
	addToCart,
	removeToCart,
});
</script>

<template>
	<Drawer v-if="drawerOpen" :currentPrice="totalPrice" :tax="priceToTax" />
	<div
		class="w-4/5 mx-auto rounded-2xl my-8 shadow-2xl p-10 flex flex-col gap-20 bg-white"
	>
		<Header :total-price="totalPrice" @open-drawer="openDrawer" />
		<router-view></router-view>
	</div>
</template>

<style scoped></style>
