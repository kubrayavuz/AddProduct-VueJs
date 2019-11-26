<template>
	<div v-if="productList.length > 0 ">
		<h3 class="text-center">Added Products</h3>
    	<hr>
		<div class="row product-container">
			<app-product v-for="(product,index) in productList">
				<img class="card-img-top" :src="product.selectedImage" :alt="product.title">
				<div class="card-body">
				 	<h5 class="card-title"> {{ product.title }}</h5>
			     	<small><strong>Quantity : </strong> {{ product.count }}</small>
			       	<br>
			    	<small><strong>Price : </strong> {{product.price}}</small>
			    	<br>
			    	<small><strong>Total : </strong> {{ product.totalPrice }}</small>			    	
			   	    <button class="btn btn-outline-danger btn-block" @click="$delete(productList,index)">Delete</button>
			    </div>
			</app-product>
		</div>
	</div>
</template>

<script>
	import {eventBus} from "../main";
	import Product from "./Product";

	export default {
		components: {
			appProduct : Product
		},
		data:function() {
			return {
				productList: []
			}
		},
		created() {
			eventBus.$on("productAdded",(product) => {
				if(this.productList.length < 10){
					this.productList.push(product);
					eventBus.$emit("progressBarUpdated", this.productList.length);
				} else  {
					alert("You cannot add more products")
				}
			});
		},
		watch: {
			productList() {
			eventBus.$emit("progressBarUpdated", this.productList.length);
			}
		}
	}
</script>

<style>
</style>