<template>
	<v-app class="pa-5">
		<v-card class="card">
			<v-card-title class="blue-grey lighten-4">
				User
				<span class="caption grey--text mt-2">&nbsp;List</span>
				<v-spacer></v-spacer>
				<!-- <v-btn class="primary white--text" to="/user/add-user">
					<v-icon left>mdi-plus-circle</v-icon>Add
				</v-btn>-->
				<v-dialog v-model="dialog" max-width="600px">
					<template v-slot:activator="{ on }">
						<v-btn v-permission="'add users'" v-on="on" class="primary white--text">
							<v-icon left>mdi-plus-circle</v-icon>Add
						</v-btn>
					</template>
					<v-card>
						<v-card-title>Add User</v-card-title>
						<v-divider></v-divider>
						<v-card-text>
							<ValidationObserver ref="nameOfObserver">
								<div class="AddUserForm">
									<label class="font-weight-bold" for="name">Name</label>
									<validation-provider name="Name" rules="required" v-slot="{ errors }">
										<input type="text" class="AddUserForm--input" v-model="form.name" />
										<span class="red--text">{{ errors[0] }}</span>
									</validation-provider>
								</div>
								<div class="AddUserForm">
									<validation-provider name="Email" rules="required|email" v-slot="{ errors }">
										<label class="font-weight-bold" for="email">Email</label>
										<input type="email" class="AddUserForm--input" v-model="form.email" />
										<span class="red--text">{{ errors[0] }}</span>
									</validation-provider>
								</div>
								<div class="AddUserForm">
									<label class="font-weight-bold" for="password">Password</label>
									<validation-provider name="Password" rules="required" v-slot="{ errors }">
										<input type="password" class="AddUserForm--input" v-model="form.password" />
										<span class="red--text">{{ errors[0] }}</span>
									</validation-provider>
								</div>
							</ValidationObserver>
						</v-card-text>
						<v-divider></v-divider>

						<v-card-actions class="text-center">
							<v-spacer>
								<v-btn color="primary" @click="closeDialog" text>
									<v-icon left>mdi-close</v-icon>Close
								</v-btn>
								<v-btn color="green" dark @click.prevent="addUser">
									<v-icon left>mdi-content-save</v-icon>Save
								</v-btn>
							</v-spacer>
						</v-card-actions>
					</v-card>
				</v-dialog>
			</v-card-title>
			<div class="pa-4">
				<div class="d-flex justify-space-between">
					<v-col md="3" cols="12" class="pa-0">
						<v-text-field outlined dense solo v-model="term" label="Search By Username"></v-text-field>
					</v-col>
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

			// addUser() {
			// 	if (this.editedIndex > -1) {
			// 		this.$axios
			// 			.$patch(`/api/user/` + this.form.id, {
			// 				name: this.form.name,
			// 				email: this.form.email,
			// 				phone: this.form.phone,
			// 				address: this.form.address,
			// 				role: this.form.role,
			// 				password: this.form.password
			// 			})
			// 			.then(res => {
			// 				this.getItems();
			// 				this.closeDialog();
			// 				this.$toast.info("Succeessfully Updated");
			// 			})
			// 			.catch(err => {
			// 				this.$refs.nameOfObserver.validate(
			// 					err.response.data.errors
			// 				);
			// 			});
			// 	} else {
			// 		this.$axios
			// 			.$post(`/api/user`, this.form)
			// 			.then(res => {
			// 				this.form = res;
			// 				this.getItems();
			// 				this.$toast.info("Succeessfully Created");
			// 				this.closeDialog();
			// 			})
			// 			.catch(err => {
			// 				this.$refs.nameOfObserver.validate(
			// 					err.response.data.errors
			// 				);
			// 				console.log(err.response.data.errors);
			// 			});
			// 	}
			// },

			addUser() {
				if (this.editedIndex > -1) {
					this.$axios
						.$patch(`/api/user/` + this.form.id, {
							name: this.form.name,
							email: this.form.email,
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