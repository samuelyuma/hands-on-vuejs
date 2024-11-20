<script lang="ts">
import { defineComponent } from 'vue';

	type Product = {
		id: number
		title: string;
		price: number
		description: string;
		image: string;
	}

	export default defineComponent({
		name: 'Products',
		data() {
			return {
				products: [] as Product[],
				isLoading: false,
				error: null as string | null,
			}
		},
		methods: {
			async fetchProduct(): Promise<void> {
				this.isLoading = true;
				this.error = null;

				try {
					const response = await fetch('https://fakestoreapi.com/products')

					if (!response.ok){
						throw new Error("Failed to fetch!")
					}

					const data: Product[] = await response.json()
					this.products = data

					console.log(data)
				} catch (error) {
					this.error = (error as Error).message || "An unknown error occured!"
				} finally {
					this.isLoading = false;
				}
			}
		},
		created() {
			this.fetchProduct()
		}
	})
</script>

<template>
  <main class="min-h-screen flex text-center items-center justify-center bg-neutral-800">

	<p v-if="isLoading" class="text-white">Loading...</p>
		<p v-if="error" class="text-white">{{ error }}</p>
		<div v-if="!isLoading && !error" class="w-full grid grid-cols-4 gap-8 p-8">
			<div v-for="product in products" :key="product.id" >
				<div class="border h-full rounded-lg">
					<img :src="product.image" :alt="product.title" class="rounded-lg">
					<p class="text-white">{{ product.title }}</p>
					<p class="text-white line-clamp-2">{{ product.description }}</p>
					<p class="text-white mt-auto">$ {{ product.price }}</p>
				</div>
			</div>
		</div>
  </main>
</template>
