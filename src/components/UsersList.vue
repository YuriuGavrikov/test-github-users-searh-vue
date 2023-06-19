<template>
	<div class="userList">
		<h3 class="title">User list:</h3>
		<TransitionGroup name="list">
			<UserItem v-for="user in users" :user="user" :key="user.id"></UserItem>
		</TransitionGroup>
	</div>
</template>

<script>
import UserItem from "./UserItem.vue";

export default {
	components: { UserItem },
	props: {
		users: {
			type: Array,
			required: true,
		},
	},
};
</script>

<style scoped>
.userList {
	display: flex;
	flex-direction: column;
	align-items: center;
}
.title {
	margin: 10px;
}
.list-enter-active,
.list-leave-active {
	transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
	opacity: 0;
	transform: translateX(30px);
}
</style>
