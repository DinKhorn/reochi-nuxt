<template>
	<v-app class="pa-5">
		<v-card class="card">
			<v-card-title class="blue-grey lighten-4">
				Stock Out
				<span class="caption grey--text mt-2">&nbsp;List</span>
				<v-spacer></v-spacer>
				<v-btn class="primary white--text" to="/stock-out/add">
					<v-icon left>mdi-plus-circle</v-icon>Add
				</v-btn>
			</v-card-title>
			<div class="pa-4">
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
		name: "UserField",
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
				name: "",
				email: "",
				term: "",
				headers: [
					{
						text: "ID",
						value: "id"
					},
					{
						text: "Name",
						value: "name"
					},
					{
						text: "Code",
						value: "code"
					},
					{
						text: "Qty",
						value: "qty",
						sortable: false
					},
					{
						text: "Deliver",
						sortable: false,
						value: "Deliver"
					},
					{
						text: "Date From",
						sortable: false,
						value: "date_from"
					},
					{
						text: "Date To",
						sortable: false,
						value: "date_to"
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
