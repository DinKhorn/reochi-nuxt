<template>
	<v-app class="pa-5">
		<v-card class="card">
			<v-card-title class="blue-grey lighten-4">
				Role
				<span class="caption grey--text mt-2">&nbsp;List</span>
				<v-spacer></v-spacer>
				<v-btn class="primary white--text" to="/outlet/add-outlet">
					<v-icon left>mdi-plus-circle</v-icon>Add
				</v-btn>
			</v-card-title>
			<div class="pa-4">
				<!-- <div class="d-flex justify-space-between">
					<v-col md="3" cols="12">
						<v-text-field outlined dense solo v-model="term" label="Search By na"></v-text-field>
					</v-col>
					<div>
						<v-btn class="red darken-1">PDF</v-btn>
						<v-btn class="lime lighten-1">CSV</v-btn>
						<v-btn class="blue lighten-1">Print</v-btn>
					</div>
				</div>-->
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

		methods: {
			getItems() {
				this.$axios
					.$get(`/api/user?name=${this.name}&email=${this.email}`)
					.then(res => {
						this.items = res.users.data;
						console.log(res);
					})
					.catch(err => {
						console.log(err.response);
					});
			},

			searchItems() {
				this.$axios
					.$get(`/api/user?term=${this.term}`)
					.then(res => {
						this.items = res.users.data;
						console.log(res);
					})
					.catch(err => {
						console.log(err.response);
					});
			},

			editItem(item) {
				this.editedIndex = this.items.indexOf(item);
				this.form = Object.assign({}, item);
				this.dialog = true;
			},

			addUser() {
				if (this.editedIndex > -1) {
					this.$axios
						.$patch(`/api/role/` + this.form.id, {
							name: this.form.name,
							email: this.form.email,
							phone: this.form.phone,
							address: this.form.address,
							role: this.form.role,
							password: this.form.password
						})
						.then(res => {
							this.getItems();
							this.closeDialog();
							this.$toast.info("Succeessfully Updated");
						})
						.catch(err => {
							this.$refs.nameOfObserver.validate(
								err.response.data.errors
							);
						});
				} else {
					this.$axios
						.$post(`/api/user`, this.form)
						.then(res => {
							this.form = res;
							this.getItems();
							this.$toast.info("Succeessfully Created");
							this.closeDialog();
						})
						.catch(err => {
							this.$refs.nameOfObserver.validate(
								err.response.data.errors
							);
							console.log(err.response.data.errors);
						});
				}
			},

			closeDialog() {
				this.dialog = false;
				this.editedIndex = -1;
				this.form = {};
			},

			deleteItem(item) {
				if (confirm("Are u sure to delete it?")) {
					this.$axios
						.$delete(`/api/user/` + item.id)
						.then(res => {
							this.getItems();
						})
						.catch(err => {
							console.log(err.response);
						});
				}
			}
		},

		created() {
			this.getItems();
		}
	};
</script>

<style lang="scss">
	.AddUserForm {
		padding-top: 10px;
		&--input {
			border: 1px solid #1232dd;
			padding: 5px 10px 5px 10px;
			width: 100%;
			outline: none;
			border-radius: 3px;
		}
	}
</style>