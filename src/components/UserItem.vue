<template>
	<div class="user">
		<div class="userTitle">
			<div class="login">Login: {{ user.login }}</div>
			<MainButton @click="showUserInfo(user.login)">
				<span v-if="showInfo">Hide</span>
				<span v-else>Show</span>
			</MainButton>
		</div>
		<div class="userInfo" v-if="showInfo">
			<div v-if="!userInfo.id">Loading...</div>
			<div v-else>
				<span>User info:</span>
				<div class="name">- Name: {{ userInfo.name }}</div>
				<div class="publicRepos">
					- Public Repositories: {{ userInfo.publicRepos }}
				</div>
				<div class="followers">- Followers: {{ userInfo.followers }}</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	props: {
		user: {
			type: Object,
			required: true,
		},
	},
	data() {
		return {
			showInfo: false,
			userInfo: {},
		};
	},
	methods: {
		showUserInfo(login) {
			this.showInfo = !this.showInfo;

			fetch(`https://api.github.com/users/${login}`)
				.then((res) => res.json())
				.then(
					(data) =>
						(this.userInfo = {
							id: data.id,
							name: data.name || "The user has no name",
							publicRepos: data.public_repos,
							followers: data.followers,
						})
				);
		},
	},
};
</script>

<style scoped>
.user {
	width: 600px;
	margin: 10px;
	padding: 10px 20px;
	background: #f0f0f0;
	border-radius: 6px;
	box-shadow: 7px 7px 5px -2px rgba(181, 189, 195, 0.5);
}
.userTitle {
	display: flex;
	justify-content: space-between;
	align-items: center;
}
.login {
	font-size: 18px;
}
</style>
