<script setup>
import { RouterLink } from "vue-router";
</script>

<template>
	<main class="flex h-screen flex-col items-center justify-around bg-black">
		<div class="max-w-md text-center">
			<h1 class="text-2xl font-semibold text-white hover:text-blue-600">Welcome to Task Management App</h1>
			
		</div>

		<div>
			<RouterLink
				to="/board"
				class="rounded-lg bg-blue-100 px-4 py-2 text-center text-3xl font-extrabold text-gray-800 transition duration-150 ease-out hover:bg-blue-600 hover:text-white"
			>
				Explore Board
			</RouterLink>
		</div>

		<div>
			<p class="text-center text-gray-600">
				Made with ❤️ by
				<a href="" class="text-blue-500 hover:text-blue-600" target="_blank">
					@Kartik
				</a>
			</p>
		</div>
	</main>
</template>
