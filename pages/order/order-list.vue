<template>
	<v-app class="pa-5">
		<v-card class="card">
			<v-card-title class="blue-grey lighten-4">
				Order
				<span class="caption grey--text mt-2">&nbsp;List</span>
				<v-spacer></v-spacer>
				<v-btn class="primary white--text" to="/order/add-order">
					<v-icon left>mdi-plus-circle</v-icon>Add
				</v-btn>
			</v-card-title>
			<div class="pa-4">
				<div class="d-flex justify-space-between">
					<div>
						<v-text-field label="Search" solo outlined dense></v-text-field>
					</div>
					<div>
						<v-btn class="red darken-1">PDF</v-btn>
						<v-btn class="lime lighten-1">CSV</v-btn>
						<v-btn class="blue lighten-1">Print</v-btn>
					</div>
				</div>
				<v-data-table :headers="headers" :items="items" v-permission="'view users'">
					<template v-slot:item.action="{item}">
						<v-tooltip top v-permission="'edit users'">
							<template v-slot:activator="{ on }">
								<v-btn icon @click="editItem(item)" color="primary" outlined v-on="on">
									<v-icon small>mdi-pencil</v-icon>
								</v-btn>
							</template>
							<span>Edit</span>
						</v-tooltip>
						<v-tooltip top v-permission="'delete users'">
							<template v-slot:activator="{ on }">
								<v-btn icon @click="deleteItem(item)" color="red" outlined v-on="on">
									<v-icon small>mdi-delete</v-icon>
								</v-btn>
							</template>
							<span>Delete</span>
						</v-tooltip>
					</template>
				</v-data-table>
			</div>
		</v-card>
	</v-app>
</template>

<script>
	export default {
		name: "StockIn",
		watch: {
			name: {
				handler() {
					this.getItems();
				}
			},

			email: {
				handler() {
					this.getItems();
				}
			},

			term: {
				handler() {
					this.searchItems();
				}
			},
			immediate: true
		},

		data() {
			return {
				dialog: false,
				items: [],
				form: {},
				headers: [
					{
						text: "Date",
						value: "date"
					},
					{
						text: "Reference No.",
						value: "reference_no"
					},
					{
						text: "Outlet Name",
						value: "outlet_name"
					},
					{
						text: "Location",
						value: "location"
					},
					{
						text: "Purchase Status",
						value: "purchase_status",
						sortable: false
					},
					{
						text: "Payment Status",
						value: "payment_status",
						sortable: false
					},
					{
						text: "Payment Due",
						value: "payment_due"
					},
					{
						text: "Order By",
						value: "order_by",
						sortable: false
					},
					{
						text: "Action",
						value: "action",
						sortable: false
					}
				],
				role: ["saleman", "administator"]
			};
		},

		methods: {}
	};
</script>
