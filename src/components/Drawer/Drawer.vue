<script setup>
import DrawerHead from './DrawerHead.vue';
import CartItemList from './CartItemList.vue';
import InfoBlock from '../infoBlock.vue';
import { ref, computed, inject } from 'vue';
import axios from 'axios';

const props = defineProps({
	currentPrice: Number,
	tax: Number,
	buttonDisabled: Boolean,
});
const { cartItems, closeDrawer } = inject('cartActions');
const orderId = ref(null);
const isCreatingOrder = ref(false);
const cartIsEmpty = computed(() => cartItems.value.length === 0);

const createOrder = async () => {
	try {
		isCreatingOrder.value = true;
		const { data } = await axios.post(
			'https://1902939eeee538b4.mokky.dev/orders',
			{
				items: cartItems.value,
				currentPrice: props.currentPrice.value,
			}
		);
		cartItems.value = [];
		orderId.value = data.id;
	} catch (err) {
		console.log(err);
	} finally {
		isCreatingOrder.value = false;
	}
};

const cartButtonDisabled = computed(
	() => isCreatingOrder.value || cartIsEmpty.value
);
</script>

<template>
	<div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>

	<div
		class="bg-white w-96 h-full fixed right-0 top-0 z-10 p-10 flex flex-col gap-6"
	>
		<DrawerHead />

		<InfoBlock
			v-if="!currentPrice && orderId"
			title="Заказ оформлен"
			:description="`Ваш
		заказ #${orderId} скоро будет передан курьерской доставке`"
			imageUrl="/order-success-icon.png"
		/>

		<InfoBlock
			v-if="!currentPrice"
			title="Корзина пустая"
			description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ"
			imageUrl="/package-icon.png"
		/>
		<CartItemList />

		<div v-if="currentPrice" class="flex flex-col gap-5 mt-auto">
			<div class="flex gap-3">
				<span>Итого:</span>
				<div class="flex-1 border-b border-dashed"></div>
				<p class="font-bold">{{ currentPrice }} р.</p>
			</div>
			<div class="flex gap-3">
				<span>Налог 5%:</span>
				<div class="flex-1 border-b border-dashed"></div>
				<p class="font-bold">{{ tax }} р.</p>
			</div>
			<button
				:disabled="cartButtonDisabled"
				@click="createOrder"
				class="transition cursor-pointer bg-lime-500 rounded-xl w-full py-3 text-white font-semibold text-base disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700"
			>
				Оформить заказ
			</button>
		</div>
	</div>
</template>
