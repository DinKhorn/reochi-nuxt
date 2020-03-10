<template>
	<v-app class="pa-5">
		<v-card class="card" id="printProduct">
			<v-card-title class="blue-grey lighten-4">Outlet Detail</v-card-title>
			<v-divider></v-divider>
			<v-row class="pa-5">
				<v-col md="4" cols="12" class="productDetail">
					<label for class="productDetail--type">Code</label>
					<span class="productDetail--item">{{ outlet.name }}</span>
				</v-col>
				<v-col md="4" cols="12" class="productDetail">
					<label for class="productDetail--type">Type</label>
					<span class="productDetail--item">{{ outlet.location }}</span>
				</v-col>
				<v-col md="4" cols="12" class="productDetail">
					<label for class="productDetail--type">Name</label>
					<span class="productDetail--item">{{ outlet.phone }}</span>
				</v-col>
				<v-col md="4" cols="12" class="productDetail">
					<label for class="productDetail--type">Brand</label>
					<span class="productDetail--item">{{ outlet.create_by }}</span>
				</v-col>
				<v-col md="4" cols="12" class="productDetail">
					<label for class="productDetail--type">Quantity</label>
					<span class="productDetail--item">{{ outlet.status }}</span>
				</v-col>
			</v-row>
			<!-- <v-btn @click="printProduct" class="mx-5 my-5 blue accent-3 white--text">
				<v-icon left>mdi-printer</v-icon>Print
			</v-btn>-->
		</v-card>
	</v-app>
</template>

<script>
	import Vue from "vue";
	var numeral = require("numeral");

	export default {
		created() {
			this.fetchData();
		},

		data() {
			return {
				outlet: []
			};
		},

		methods: {
			fetchData() {
				this.$axios
					.$get(`api/outlets/` + this.$route.params.id)
					.then(res => {
						this.outlet = res.outlet;
						// console.log(this.outlet);
					})
					.catch(err => {
						console.log(err.response);
					});
			}

			// printProduct() {
			// 	this.$htmlToPaper("printProduct");
			// }
		}
	};
</script>

<style lang="scss">
	.productDetail {
		display: flex;
		flex-direction: column;
		&--type {
			font-weight: 500;
		}
		&--item {
			font-size: 15px;
		}
	}
</style>