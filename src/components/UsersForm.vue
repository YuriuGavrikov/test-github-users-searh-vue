<template>
	<form class="search" @submit.prevent>
		<input
			v-model="login"
			class="input"
			type="text"
			placeholder="Enter login"
		/>
		<MainButton @click="this.searchUsers(login)">Search</MainButton>
	</form>
</template>

<script>
export default {
	props: {
		fetchUsers: {
			type: Function,
			required: true,
		},
	},
	data() {
		return {
			login: "",
		};
	},
	methods: {
		searchUsers(login) {
			if (login) {
				this.fetchUsers(login);
				this.login = "";
			} else {
				alert("Enter login");
			}
		},
	},
};
</script>

<style scoped>
.search {
	width: 600px;
	margin: 10px;
	padding: 10px;
	background: #f0f0f0;
	border-radius: 6px;
	box-shadow: 7px 7px 5px -2px rgba(181, 189, 195, 0.5);
	display: flex;
	justify-content: space-between;
}
.input {
	padding: 10px;
	width: 80%;
	outline: none;
	border: none;
	border-radius: 6px;
	box-shadow: 5px 5px 5px -2px rgba(181, 189, 195, 0.5);
}
</style>
