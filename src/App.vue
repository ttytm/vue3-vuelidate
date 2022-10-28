<script setup lang="ts">
import { reactive, computed } from "vue";
import { useVuelidate } from "@vuelidate/core";
import { required, minLength, email, sameAs, not } from "@vuelidate/validators";

const formData = reactive({
	username: "",
	sameAsComputed: "",
	notSameAsComputed: "",
	notSameAsStatic: "",
	email: "",
	password: "",
});

const rules = computed(() => ({
	username: {
		required,
		minLength: minLength(6),
	},
	sameAsComputed: {
		notSameAs: sameAs(formData.username),
	},
	notSameAsComputed: {
		notSameAs: not(sameAs(formData.username)),
	},
	notSameAsStatic: {
		notSameAs: not(sameAs("asdf")),
	},
	email: {
		required,
		email,
	},
	password: {
		required,
		minLength: minLength(8),
	},
}));

const v$ = useVuelidate(rules, formData);
</script>

<template class="bg-slate-800">
	<div class="mx-auto flex space-x-8 justify-center mt-20">
		<form class="w-80 mt-4">
			<!-- Username -->
			<div class="mb-6">
				<label for="email" class="block mb-2 text-sm font-medium">Username</label>
				<input
					type="text"
					id="username"
					v-model.trim="v$.username.$model"
					class="bg-gray-50 border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-80 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 focus-visible:outline-none dark:focus:border-blue-500"
					placeholder="Username"
				/>
				<template v-if="v$.username.$error">
					<div v-for="(error, index) in v$.username.$errors" :key="index" class="text-sm text-red-400 mt-2">
						{{ error.$message }}
					</div>
				</template>
			</div>

			<!-- Same As Computed -->
			<div class="mb-6">
				<label for="email" class="block mb-2 text-sm font-medium"
					>Same As Computed (Username) <span class="text-xs text-green-400">works</span></label
				>
				<input
					type="text"
					id="notSameAsComputed"
					v-model.trim="v$.sameAsComputed.$model"
					class="bg-gray-50 border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-80 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 focus-visible:outline-none dark:focus:border-blue-500"
					placeholder="Username"
				/>
				<template v-if="v$.sameAsComputed.$error">
					<div v-for="(error, index) in v$.sameAsComputed.$errors" :key="index" class="text-sm text-red-400 mt-2">
						{{ error.$message }}
					</div>
				</template>
			</div>

			<!-- Not Same As Computed -->
			<div class="mb-6">
				<label for="email" class="block mb-2 text-sm font-medium"
					>Not Same As Computed (Username) <span class="text-xs text-red-400">!broken</span></label
				>
				<input
					type="text"
					id="notSameAsComputed"
					v-model.trim="v$.notSameAsComputed.$model"
					class="bg-gray-50 border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-80 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:focus:ring-blue-500 dark:focus:border-blue-500 focus-visible:outline-none"
					placeholder="Username"
				/>
				<p class="mt-2 text-sm">Equal: Should throw error but doesn't</p>
				<template v-if="v$.notSameAsComputed.$error">
					<div v-for="(error, index) in v$.notSameAsComputed.$errors" :key="index" class="text-sm text-red-400 mt-2">
						{{ error.$message }}
					</div>
				</template>
			</div>

			<!-- Not Same As Static -->
			<div class="mb-6">
				<label for="email" class="block mb-2 text-sm font-medium"
					>Not Same As Static ("asdf") <span class="text-xs text-green-400">works</span></label
				>
				<input
					type="text"
					id="notSameAsComputed"
					v-model.trim="v$.notSameAsStatic.$model"
					class="bg-gray-50 border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-80 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 focus-visible:outline-none dark:focus:border-blue-500"
					placeholder="Username"
				/>
				<template v-if="v$.notSameAsStatic.$error">
					<div v-for="(error, index) in v$.notSameAsStatic.$errors" :key="index" class="text-sm text-red-400 mt-2">
						{{ error.$message }}
					</div>
				</template>
			</div>

			<!-- Mail -->
			<div class="mb-6">
				<label for="email" class="block mb-2 text-sm font-medium">Email</label>
				<input
					type="email"
					id="email"
					v-model.trim="v$.email.$model"
					class="bg-gray-50 border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-80 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 focus-visible:outline-none dark:focus:border-blue-500"
					placeholder="name@domain.com"
					required
				/>

				<template v-if="v$.email.$error">
					<div v-for="(error, index) in v$.email.$errors" :key="index" class="text-sm text-red-400 mt-2">
						{{ error.$message }}
					</div>
				</template>
			</div>

			<!-- Password -->
			<div class="mb-6">
				<label for="password" class="block mb-2 text-sm font-medium"> Password </label>
				<input
					type="password"
					id="password"
					v-model.trim="v$.password.$model"
					class="bg-gray-50 border border-gray-300 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-80 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 focus-visible:outline-none dark:focus:border-blue-500"
				/>
				<template v-if="v$.password.$error">
					<div v-for="(error, index) in v$.password.$errors" :key="index" class="text-sm text-red-400 mt-2">
						{{ error.$message }}
					</div>
				</template>
			</div>

			<button
				type="submit"
				class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
			>
				Submit
			</button>
		</form>

		<div
			class="p-4 max-w-4xl bg-white rounded-lg border border-gray-200 shadow-md dark:bg-gray-800 dark:border-gray-700"
		>
			<h5 class="mb-2 text-xl font-bold tracking-tight">Debug $v</h5>
			<div v-for="(_, index) in rules" :key="index" class="mt-2 text-sm">
				<div class="items-center">
					<p class="font-semibold">{{ index }}:</p>
					<p>{{ v$[index] }}</p>
				</div>
			</div>
		</div>
	</div>
</template>
