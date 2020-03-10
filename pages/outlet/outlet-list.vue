<template>
	<v-app class="pa-5">
		<v-card class="card">
			<v-card-title class="blue-grey lighten-4">
				Outlet
				<span class="caption grey--text mt-2">&nbsp;List</span>
				<v-spacer></v-spacer>
				<v-btn class="primary white--text" to="/outlet/add-outlet">
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
				<v-data-table :headers="headers" :items="items" :items-per-page="itemsPerPage">
					<template v-slot:item="{ item }">
						<tr>
							<td>{{ item.id }}</td>
							<td>{{ item.name }}</td>
							<td>{{ item.location }}</td>
							<td>{{ item.phone }}</td>
							<td>{{ item.create_by }}</td>
							<td>{{ item.status }}</td>
							<td>
								<v-tooltip top v-permission="'edit sales'">
									<template v-slot:activator="{ on }">
										<v-btn icon small @click="viewItem(item.id)" color="cyan" outlined v-on="on">
											<v-icon small>mdi-eye</v-icon>
										</v-btn>
									</template>
									<span>View</span>
								</v-tooltip>
								<v-tooltip top v-permission="'edit sales'">
									<template v-slot:activator="{ on }">
										<v-btn icon small @click="editItem(item.id)" color="primary" outlined v-on="on">
											<v-icon small>mdi-pencil</v-icon>
										</v-btn>
									</template>
									<span>Edit</span>
								</v-tooltip>
								<v-tooltip top v-permission="'delete sales'">
									<template v-slot:activator="{ on }">
										<v-btn icon small @click="deleteItem(item)" color="red" outlined v-on="on">
											<v-icon small>mdi-delete</v-icon>
										</v-btn>
									</template>
									<span>Delete</span>
								</v-tooltip>
							</td>
						</tr>
					</template>
				</v-data-table>
			</div>
		</v-card>
	</v-app>
</template>


<script>
	import Vue from "vue";
	var numeral = require("numeral");

	Vue.filter("formatNumber", function(value) {
		return numeral(value).format("0,0.00");
	});

	export default {
		name: "Outlet",

		created() {
			this.fetchData();
		},

		watch: {
			options: {
				handler() {
					this.fetchData();
				}
			}
		},

		data() {
			return {
				items: [],
				search: "",
				form: {},
				page: 1,
				options: {},
				itemsPerPage: 5,
				editedIndex: -1,
				created: true,
				dialog: false,
				dialog2: false,
				headers: [
					{
						text: "No.",
						value: "id"
					},
					{
						text: "Name",
						value: "name"
					},
					{
						text: "Location",
						value: "location"
					},
					{
						text: "phone",
						value: "phone"
					},
					{
						text: "Create By",
						value: "create_by"
					},
					{
						text: "Status",
						value: "status"
					},
					{
						text: "Actions",
						sortable: false,
						value: "action"
					}
				]
			};
		},

		methods: {
			fetchData() {
				let vm = this;
				this.$axios
					.$get(
						`/api/outlets?temsPerPage=${this.options.itemsPerPage}&page=${this.options.page}`
					)
					.then(res => {
						vm.items = res.outlets.data;
						console.log(res);
					})
					.catch(err => {
						console.log(err);
					});
			},

			editItem(id) {
				this.$router.push(`/outlet/outlet-list/${id}/edit`);
			},

			viewItem(id) {
				this.$router.push(`/outlet/outlet-list/${id}`);
			},

			deleteItem(item) {
				this.$axios
					.$delete(`api/outlets/` + item.id)
					.then(res => {
						this.fetchData();
					})
					.catch(err => {
						// console.log(err.response);
					});
			}
		}
	};
</script>
<style lang="scss">
	.menu-list {
		background-color: #34495e;
		color: #fff;
	}

	.nuxt--link {
		display: block;
		text-decoration: none;
	}

	.form-control {
		width: 100%;
		padding-bottom: 5px;
		padding-top: 5px;
		padding-right: 10px;
		padding-left: 10px;
		outline: none;
		border-radius: 5px;
		border: 1px solid #616161;
	}
</style>
