<template>
	<select class="select" @change="changeOption" v-model="this.valueSort">
		<option disabled value="">Sort by</option>
		<option
			v-for="option in options"
			:key="option.value"
			:value="option.value"
		>
			{{ option.value }}
		</option>
	</select>
</template>

<script>
export default {
	name: "MainSelect",
	props: {
		lastLogin: {
			type: String,
			required: true,
		},
		options: {
			type: Array,
			default: () => [],
		},
		fetchUsers: {
			type: Function,
			required: true,
		},
	},
	data() {
		return {
			valueSort: "",
		};
	},
	methods: {
		sortUsers(lastLogin, valueSort) {
			if (lastLogin) {
				valueSort === "repo max"
					? this.fetchUsers(lastLogin, "desc")
					: this.fetchUsers(lastLogin, "asc");
			}
		},
	},
	watch: {
		valueSort(newValue) {
			this.sortUsers(this.lastLogin, newValue);
		},
	},
};
</script>

<style scoped>
.select {
	width: 160px;
	height: 40px;
	padding: 10px;
	margin: 5px;
	border: 3px solid rgba(181, 189, 195, 0.5);
	border-radius: 3px;
	outline: none;
}
</style>
