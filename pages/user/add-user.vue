<template>
	<v-app class="pa-5">
		<v-card class="card">
			<v-card-title class="blue-grey lighten-4">Create User</v-card-title>
			<v-divider></v-divider>
			<p class="px-5 pt-3 font-italic grey--text">
				The field labels marked with
				<span class="red--text">*</span> are required input fields.
			</p>
			<ValidationObserver ref="form">
				<v-row class="px-5">
					<v-col sm="4" cols="12">
						<label class="font-weight-bold" for="name">
							Name
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Name" rules="required" v-slot="{ errors }">
							<v-text-field outlined solo dense label="Name" v-model="form.name"></v-text-field>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>
					<!-- <v-col sm="4" cols="12">
						<label class="font-weight-bold" for="address">
							Address
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Address" rules="required" v-slot="{ errors }">
							<v-text-field outlined solo dense label="Address" v-model="form.address"></v-text-field>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>-->
					<!-- <v-col sm="4" cols="12">
						<label class="font-weight-bold" for="phone">
							Phone
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Phone" rules="required" v-slot="{ errors }">
							<v-text-field outlined solo dense label="Phone" v-model="form.phone"></v-text-field>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>-->
					<v-col sm="4" cols="12">
						<label class="font-weight-bold" for="email">
							Email
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Email" rules="required" v-slot="{ errors }">
							<v-text-field outlined solo dense label="Email" v-model="form.email"></v-text-field>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>
					<v-col sm="4" cols="12">
						<label class="font-weight-bold" for="password">
							Password
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Password" rules="required" v-slot="{ errors }">
							<v-text-field outlined solo dense label="Password" v-model="form.password"></v-text-field>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>
					<!-- <v-col sm="4" cols="12">
						<label class="font-weight-bold" for="Role">
							Role
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Role" rules="required" v-slot="{ errors }">
							<v-select :items="role" v-model="form.role" outlined solo dense label="Select Role"></v-select>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>-->
				</v-row>
			</ValidationObserver>
			<v-card-actions class="px-5">
				<v-btn color="primary" @click.prevent="createItem">
					<v-icon>mdi-check</v-icon>Create
				</v-btn>
			</v-card-actions>
		</v-card>
	</v-app>
</template>

<script>
	export default {
		name: "AddOutlet",
		data() {
			return {
				form: {},
				items: [],
				url: null,
				itemsPerPage: 5
			};
		},

		methods: {
			createItem() {
				this.$axios
					.$post(`/api/user`, this.form)
					.then(res => {
						this.form = res;
						this.getItems();
						this.$toast.info("Succeessfully Created");
						this.closeDialog();
					})
					.catch(err => {
						// this.$refs.nameOfObserver.validate(
						err.response.data.errors;
						// );
						console.log(err.response.data.errors);
					});
			}
		}
	};
</script>
