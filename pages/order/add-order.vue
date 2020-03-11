<template>
	<v-app class="pa-5">
		<v-card class="card">
			<v-card-title class="blue-grey lighten-4">Create Order</v-card-title>
			<p class="px-5 pt-3 font-italic grey--text">
				The field labels marked with
				<span class="red--text">*</span> are required input fields.
			</p>
			<ValidationObserver ref="form">
				<v-row class="px-5">
					<v-col sm="6" cols="12">
						<label class="font-weight-bold" for="name">
							Product Name
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Product Name" rules="required" v-slot="{ errors }">
							<v-autocomplete
								:items="products"
								item-text="name"
								outlined
								solo
								dense
								label="Product Name"
								v-model="form.name"
							></v-autocomplete>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>
					<v-col sm="6" cols="12">
						<label class="font-weight-bold" for="code">
							Code
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Code" rules="required" v-slot="{ errors }">
							<v-autocomplete
								:items="products"
								item-text="code"
								outlined
								solo
								dense
								label="Product Code"
								v-model="form.code"
							></v-autocomplete>
							<!-- <v-text-field outlined solo dense label="Code"></v-text-field> -->
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>
					<v-col sm="6" cols="12">
						<label class="font-weight-bold" for="qty">
							Qty
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Qty" rules="required" v-slot="{ errors }">
							<v-text-field outlined solo dense label="Qty" v-model="form.qty" type="number"></v-text-field>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>
					<v-col sm="6" cols="12">
						<label class="font-weight-bold" for="supplier">
							Supplier
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Supplier" rules="required" v-slot="{ errors }">
							<v-autocomplete
								:items="suppliers"
								item-text="name"
								outlined
								solo
								dense
								label="Supplier Name"
								v-model="form.supplier"
							></v-autocomplete>
							<!-- <v-text-field outlined solo dense label="Supplier" v-model="form.supplier"></v-text-field> -->
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>
					<v-col sm="6" cols="12">
						<label class="font-weight-bold" for="date_from">
							Date From
							<span class="red--text">*</span>
						</label>
						<v-menu
							v-model="menu1"
							:close-on-content-click="false"
							:nudge-right="40"
							transition="scale-transition"
							offset-y
							min-width="290px"
						>
							<template v-slot:activator="{ on }">
								<v-text-field v-model="date" readonly v-on="on" outlined solo dense></v-text-field>
							</template>
							<v-date-picker v-model="date" @input="menu1 = false"></v-date-picker>
						</v-menu>
					</v-col>
					<v-col sm="6" cols="12">
						<label class="font-weight-bold" for="date_to">
							Date To
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Date To" rules="required" v-slot="{ errors }">
							<v-menu
								v-model="menu2"
								:close-on-content-click="false"
								:nudge-right="40"
								transition="scale-transition"
								offset-y
								min-width="290px"
							>
								<template v-slot:activator="{ on }">
									<v-text-field v-model="date" readonly outlined solo dense v-on="on"></v-text-field>
								</template>
								<v-date-picker v-model="date" @input="menu2 = false"></v-date-picker>
							</v-menu>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>
				</v-row>
			</ValidationObserver>
			<v-card-actions class="px-5 pb-5">
				<v-btn color="primary" @click.prevent="createItem">
					<v-icon>mdi-check</v-icon>Create
				</v-btn>
			</v-card-actions>
		</v-card>
	</v-app>
</template>

<script>
	export default {
		name: "StockIn",

		data() {
			return {
				date: new Date().toISOString().substr(0, 10),
				menu1: false,
				modal: false,
				menu2: false,
				form: {},
				// items: [],
				products: [],
				suppliers: [],
				itemsPerPage: 5,
				role: ["saleman", "administator"]
			};
		},

		created() {
			this.initialize();
		},

		methods: {
			initialize() {
				this.$axios.get(`api/product`).then(res => {
					this.products = res.data.products.data;
				});

				this.$axios
					.$get(`api/supplier`)
					.then(res => {
						this.suppliers = res.suppliers.data;
					})
					.catch(err => {
						console.log(err.response);
					});
			}
		}
	};
</script>
