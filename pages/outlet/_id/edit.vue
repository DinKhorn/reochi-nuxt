<template>
	<v-app class="pa-5">
		<v-card class="card">
			<v-card-title class="blue-grey lighten-4">Edit Outlet</v-card-title>
			<v-divider></v-divider>
			<p class="px-5 pt-3 font-italic grey--text">
				The field labels marked with
				<span class="red--text">*</span> are required input fields.
			</p>
			<ValidationObserver ref="form">
				<v-row class="px-5">
					<v-col sm="4" cols="12">
						<label class="font-weight-bold" for="name">
							Outlet Name
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Name" rules="required" v-slot="{ errors }">
							<v-text-field outlined solo dense label="Outlet Name" v-model="form.name"></v-text-field>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>
					<v-col sm="8" cols="12">
						<label class="font-weight-bold" for="location">
							Location
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Location" rules="required" v-slot="{ errors }">
							<v-text-field outlined solo dense label="Location" v-model="form.location"></v-text-field>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>
					<v-col sm="4" cols="12">
						<label class="font-weight-bold" for="phone">
							Phone
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Phone" rules="required" v-slot="{ errors }">
							<v-text-field outlined solo dense label="Phone" v-model="form.phone"></v-text-field>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>
					<v-col sm="4" cols="12">
						<label class="font-weight-bold" for="create_by">
							Create By
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Create By" rules="required" v-slot="{ errors }">
							<v-text-field outlined solo dense label="Create By" v-model="form.create_by"></v-text-field>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>
					<v-col sm="4" cols="12">
						<label class="font-weight-bold" for="status">
							Status
							<span class="red--text">*</span>
						</label>
						<validation-provider name="Status" rules="required" v-slot="{ errors }">
							<v-text-field outlined solo dense label="Status" v-model="form.status"></v-text-field>
							<span class="red--text">{{ errors[0] }}</span>
						</validation-provider>
					</v-col>
				</v-row>
			</ValidationObserver>
			<v-card-actions class="px-5">
				<v-btn color="primary" @click.prevent="updateItem">
					<v-icon>mdi-check</v-icon>Update
				</v-btn>
			</v-card-actions>
		</v-card>
	</v-app>
</template>

<script>
	import moment from "moment";
	export default {
		name: "EditOutlet",
		created() {
			this.setData();
		},

		data() {
			return {
				form: {},
				items: [],
				itemsPerPage: 5
			};
		},

		methods: {
			setData() {
				this.$axios
					.$get(`api/outlets/` + this.$route.params.id)
					.then(res => {
						this.$set(this.$data, "form", res.outlet);
						console.log(this.$data);
					})
					.catch(err => {
						console.log(err.response);
					});
			},

			updateItem() {
				this.$axios
					.$patch(`api/outlets/` + this.form.id, {
						name: this.form.name,
						location: this.form.location,
						phone: this.form.phone,
						create_by: this.form.create_by,
						status: this.form.status
					})
					.then(res => {
						this.items = res.data;
						this.$toast.info("Succeessfully updated");
						this.$router.push("/outlet/outlet-list");
					})
					.catch(err => {
						this.$refs.form.validate(err.response.data.errors);
					});
			}
		}
	};
</script>