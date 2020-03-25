<template>
	<v-app>
		<v-app-bar color="green darken-3" app fixed clipped dense dark>
			<v-app-bar-nav-icon @click.stop="toggle = !toggle"></v-app-bar-nav-icon>
			<v-toolbar-title>REOCHI</v-toolbar-title>
			<v-spacer></v-spacer>
			<v-toolbar-items>
				<!-- <nuxt-link v-permission="'pos'" style="color: #fff" class="posLink" to="/sale/pos/create">
					<v-btn text dark class="posLink--title">
						<v-icon left>mdi-cart</v-icon>POS
					</v-btn>
				</nuxt-link>-->

				<v-btn text dark>
					<a href="/user/profile" class="nuxt--link">{{ user.user ? user.user.name : null }}</a>
				</v-btn>
				<v-btn text dark @click="logout()">
					<v-icon>mdi-logout</v-icon>
				</v-btn>

				<!-- <v-list-group>
					<template v-slot:activator>
						<v-list-item-content>
							<v-list-item-title>{{ user.user ? user.user.name : null }}</v-list-item-title>
						</v-list-item-content>
					</template>
					<v-list-item style="background-color:white;">
						<v-list-item-content>
							<v-list-item-title class="red--text">Profile</v-list-item-title>
						</v-list-item-content>
						<v-list-item-content>
							<v-list-item-title>
								<v-btn text dark @click="logout()">
									<v-icon class="red--text">mdi-logout</v-icon>
								</v-btn>
							</v-list-item-title>
						</v-list-item-content>
					</v-list-item>
				</v-list-group>-->
			</v-toolbar-items>
		</v-app-bar>

		<v-navigation-drawer app clipped-left class="color" dense v-model="toggle">
			<v-list-item class="text-center font-weight-bold">
				<v-list-item-content style="padding:0px;">
					<v-img :src="require('@/assets/img/logo2.png')" height="48" />
				</v-list-item-content>
			</v-list-item>

			<v-divider></v-divider>

			<v-list dense rounded>
				<div v-for="(item, i) in menus" :key="i" router exact>
					<div v-if="$laravel.hasPermission(item.permission)">
						<template v-if="!item.children">
							<v-list-item :key="i" :to="item.to">
								<v-list-item-content>
									<v-list-item-title>
										<v-icon left>{{ item.icon }}</v-icon>
										{{ item.name }}
									</v-list-item-title>
								</v-list-item-content>
							</v-list-item>
						</template>
						<template v-else>
							<v-list-group :key="i.name" :value="false">
								<template v-slot:activator>
									<v-list-item-content>
										<v-list-item-title>
											<v-icon left>{{ item.icon }}</v-icon>
											{{ item.name }}
										</v-list-item-title>
									</v-list-item-content>
								</template>
								<template v-for="(subMenu, i) in item.children">
									<v-list-item :to="subMenu.to" :key="i" exact class="subMenu">
										<v-list-item-content>
											<v-list-item-title>
												<v-icon left>{{ subMenu.icon }}</v-icon>
												{{ subMenu.name }}
											</v-list-item-title>
										</v-list-item-content>
									</v-list-item>
								</template>
							</v-list-group>
						</template>
					</div>
				</div>
			</v-list>
		</v-navigation-drawer>

		<v-content>
			<nuxt />
		</v-content>
	</v-app>
</template>

<script>
	export default {
		data() {
			return {
				toggle: true,
				menus: [
					{
						name: "Dashboard",
						icon: "mdi-view-dashboard",
						to: "/",
						permission: "view users"
					},
					{
						name: "User Management",
						icon: "mdi-account-group",
						permission: "view product",
						children: [
							{
								name: "User",
								to: "/user/user-list",
								icon: "mdi-account",
								permission: "view product"
							},
							{
								name: "Salesman",
								to: "/salesman/list",
								icon: "mdi-account-tie",
								permission: "view product"
							},
							{
								name: "Supplier",
								to: "/supplier/",
								icon: "mdi-account-multiple",
								permission: "view supplier"
							},
							{
								name: "Role",
								icon: "mdi-account-cog",
								to: "/role/role-list",
								permission: "view users"
							}
						]
					},
					{
						name: "Outlet",
						icon: "mdi-storefront",
						permission: "view product",
						children: [
							{
								name: "Outlet List",
								to: "/outlet/outlet-list",
								icon: "mdi-view-list",
								permission: "view product"
							},
							{
								name: "Add Outlet",
								to: "/outlet/add-outlet",
								icon: "mdi-plus-circle",
								permission: "view product"
							}
						]
					},
					{
						name: "Product",
						icon: "mdi-cube",
						permission: "view product",
						children: [
							{
								name: "Product List",
								to: "/product/product-list",
								icon: "mdi-view-list",
								permission: "view product"
							},
							{
								name: "Add Product",
								to: "/product/add-product",
								icon: "mdi-plus-circle",
								permission: "view product"
							},
							{
								name: "Product Category",
								to: "/product/category/",
								icon: "mdi-apps",
								permission: "view product"
							},
							{
								name: "Add Category",
								to: "/product/category/add_category",
								icon: "mdi-plus-circle",
								permission: "view product"
							}
						]
					},

					{
						name: "Order",
						icon: "mdi-cart-arrow-down",
						permission: "view purchase",
						children: [
							{
								name: "Order List",
								to: "/order/order-list",
								icon: "mdi-view-list",
								permission: "view purchase"
							},
							{
								name: "Add Order",
								to: "/order/add-order",
								icon: "mdi-plus-circle",
								permission: "view purchase"
							},
							{
								name: "Add Purchase",
								to: "/purchase/add-purchase",
								icon: "mdi-plus-circle",
								permission: "view purchase"
							}
						]
					},
					{
						name: "Stock In",
						icon: "mdi-clipboard-arrow-down-outline",
						permission: "view users",
						children: [
							{
								name: "Stockin List",
								to: "/stock-in/list",
								icon: "mdi-view-list",
								permission: "view users"
							},
							{
								name: "Add Stock",
								to: "/stock-in/add",
								icon: "mdi-plus-circle",
								permission: "view users"
							}
						]
					},
					{
						name: "Stock Out",
						icon: "mdi-clipboard-arrow-up-outline",
						permission: "view users",
						children: [
							{
								name: "Stock-out List",
								to: "/stock-out/list",
								icon: "mdi-view-list",
								permission: "view users"
							},
							{
								name: "Cut Stock",
								to: "/stock-out/add",
								icon: "mdi-plus-circle",
								permission: "view users"
							}
						]
					},
					{
						name: "Transfer",
						icon: "mdi-transfer-right",
						permission: "view transfer",
						children: [
							{
								name: "Transfer List",
								icon: "mdi-view-list",
								to: "/transfer/transfers",
								permission: "view transfer"
							},
							{
								name: "Add Transfer",
								icon: "mdi-plus-circle",
								to: "/transfer/add_transfer",
								permission: "view transfer"
							},
							{
								name: "Import Transfer By CSV",
								icon: "mdi-file-import",
								to: "/transfer/import_transfer",
								permission: "view transfer"
							}
						]
					},
					{
						name: "Location",
						icon: "mdi-google-maps",
						permission: "view purchase",
						children: [
							{
								name: "Location List",
								to: "/location/",
								icon: "mdi-view-list",
								permission: "view purchase"
							},
							{
								name: "Add Location",
								to: "/location/create",
								icon: "mdi-plus-circle",
								permission: "view purchase"
							}
						]
					},
					{
						name: "System Settings",
						icon: "mdi-cogs",
						to: "/setting/system-settings",
						permission: "view users"
					}
				]
			};
		},

		methods: {
			logout() {
				this.$auth.logout();
			}
		},

		computed: {
			user() {
				return this.$store.state.auth ? this.$store.state.auth.user : {};
			}
		},

		// async created() {
		// 	const { data: permissions } = await this.$axios.get(
		// 		"/api/auth/permissions"
		// 	);
		// 	const { data: roles } = await this.$axios.get("/api/auth/roles");

		// 	this.$laravel.setPermissions(permissions);
		// 	this.$laravel.setRoles(roles);
		// 	console.log(roles, permissions);
		// }

		created() {
			if (this.user.role[0] === "administrator") {
				this.$laravel.setPermissions([
					"view users",
					"add users",
					"edit users",
					"delete users",
					"view sales",
					"add sales",
					"import sales",
					"edit sales",
					"delete sales",
					"view website",
					"add website",
					"edit website",
					"delete website",
					"view product",
					"add product",
					"edit product",
					"delete product",
					"import product",
					"view purchase",
					"add purchase",
					"edit purchase",
					"delete purchase",
					"import purchase",
					"view expense",
					"add expense",
					"edit expense",
					"delete expense",
					"import expense",
					"view quotation",
					"add quotation",
					"edit quotation",
					"view transfer",
					"add transfer",
					"edit transfer",
					"view employee",
					"add employee",
					"edit employee",
					"delete employee",
					"view account",
					"add account",
					"edit account",
					"delete account",
					"import account",
					"view customer",
					"add customer",
					"edit customer",
					"delete customer",
					"import customer",
					"view biller",
					"add biller",
					"edit biller",
					"delete biller",
					"import biller",
					"view supplier",
					"add supplier",
					"edit supplier",
					"delete supplier",
					"import supplier",
					"summary report",
					"product report",
					"daily sale report",
					"monthly sale report",
					"daily purchase report",
					"monthly purchase report",
					"sale report",
					"payment report",
					"purchase report",
					"warehouse report",
					"product qty alert",
					"user report",
					"customer report",
					"supplier report",
					"due report"
					// "support",
					// "payment log",
					// "add withdraw method",
					// "view withdraw method",
					// "edit withdraw method",
					// "delete withdraw method",
					// "charge commission",
					// "pos",
					// "return sales",
					// "return purchases"
				]);
			}

			if (
				this.user.role[0] === "saleman" ||
				this.user.role[0] === "Supervisor"
			) {
				this.$laravel.setPermissions([
					"view sales",
					"add sales",
					"edit sales",
					"delete sales",
					"import sales",
					"summary report",
					"product report",
					"daily sale report",
					"monthly sale report",
					"daily purchase report",
					"sale report",
					"purchase report",
					"warehouse report",
					"product qty alert",
					"customer report",
					"supplier report",
					"due report",
					"pos",
					"return sales"
				]);
			}

			if (this.user.role[0] === "accountant") {
				this.$laravel.setPermissions([
					"view account",
					"add account",
					"edit account",
					"delete account",
					"import account",
					"view expense",
					"add expense",
					"edit expense",
					"delete expense",
					"import expense"
				]);
			}

			if (this.user.role[0] === "saleManager") {
				this.$laravel.setPermissions([
					"view sales",
					"import sales",
					"view product",
					"payment log",
					"add withdraw method",
					"view expense",
					"return sales",
					"return purchases",
					"summary report",
					"product report",
					"daily sale report",
					"monthly sale report",
					"daily purchase report",
					"monthly purchase report",
					"sale report",
					"payment report",
					"purchase report",
					"warehouse report",
					"product qty alert",
					"user report",
					"customer report",
					"supplier report",
					"due report"
				]);
			}
		}
	};
</script>


<style lang="scss">
	.color {
		background: #34495e;
		&--item {
			background: #455a64;
		}
	}

	.subMenu {
		padding-left: 2em;
	}

	.posLink {
		display: block;
		text-decoration: none;
		padding-top: 7px;
	}
	.nuxt--link {
		display: block;
		text-decoration: none;
		color: #fff !important;
	}
</style>