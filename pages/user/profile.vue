<template>
	<v-app class="pa-5">
		<v-card class="card">
			<v-card-title class="blue-grey lighten-4">
				User Profile
				<!-- <span class="caption grey--text mt-2">&nbsp;List</span> -->
				<v-spacer></v-spacer>
				<!-- <v-btn class="primary white--text" to="/user/add-user">
					<v-icon left>mdi-plus-circle</v-icon>Add
				</v-btn>-->
			</v-card-title>
			<div class="pa-4">Coming Soon</div>
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
						value: "id",
						sortable: false
					},
					{
						text: "Name",
						value: "name",
						sortable: false
					},
					{
						text: "Email",
						value: "email",
						sortable: false
					},
					{
						text: "Phone",
						sortable: false,
						value: "phone"
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
