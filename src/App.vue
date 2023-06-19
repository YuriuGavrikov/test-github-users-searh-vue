<template>
	<div class="app">
		<h2 class="title">Search user Github</h2>
		<UsersForm :fetchUsers="fetchUsers"></UsersForm>
		<MainSelect
			:fetchUsers="fetchUsers"
			:lastLogin="lastLogin"
			:options="sortOptions"
		/>
		<UsersList :users="users"></UsersList>
		<div class="observer" ref="observer">---</div>
	</div>
</template>

<script>
import UsersForm from "@/components/UsersForm.vue";
import UsersList from "@/components/UsersList.vue";

export default {
	components: {
		UsersForm,
		UsersList,
	},
	data() {
		return {
			users: [],
			lastLogin: "",
			sortOptions: [{ value: "repo max" }, { value: "repo min" }],
			nextUrl: "",
		};
	},
	methods: {
		fetchUsers(login, valueSort) {
			this.users = [];
			this.lastLogin = login;

			fetch(
				`https://api.github.com/search/users?q=${login}+sort:repositories-${valueSort}&per_page=10`
			)
				.then((res) => {
					this.nextUrl = res.headers
						.get("Link")
						.match(/(?<=<)([\S]*)(?=>; rel="Next")/i)[0];
					return res.json();
				})
				.then((data) => data.items)
				.then((items) =>
					items.forEach((item) => {
						this.users.push({
							id: item.id,
							login: item.login,
						});
					})
				);
		},
		loadUsers(url) {
			if (this.users.length === 0) {
				return null;
			} else {
				fetch(url)
					.then((res) => res.json())
					.then((data) =>
						data.items.forEach((item) => {
							this.users.push({
								id: item.id,
								login: item.login,
							});
						})
					);
			}
		},
	},
	mounted() {
		let options = {
			rootMargin: "0px",
			threshold: 1.0,
		};

		const callback = (entries, observer) => {
			if (entries[0].isIntersecting && this.users) {
				this.loadUsers(this.nextUrl);
			}
		};

		let observer = new IntersectionObserver(callback, options);
		observer.observe(this.$refs.observer);
	},
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@300&display=swap");
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: "Roboto", sans-serif;
}
.app {
	display: flex;
	flex-direction: column;
	align-items: center;
}
.title {
	margin: 15px;
}
.observer {
	width: 100%;

	text-align: center;
}
</style>
