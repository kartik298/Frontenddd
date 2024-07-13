<script setup>
import { RouterLink } from "vue-router";
</script>

<template>
	<header class="flex shrink-0 justify-between bg-gray-800 px-4 py-3">
    <button class="px-10">
      <img
        src="@/assets/profile.png"
        alt="User Avatar"
        class=" h-9 w-9 rounded-full"
        width="36"
        height="20"
      />
    </button>
    <RouterLink class="py-4 text-white tracking-tight" to="/">Kanbaan Board</RouterLink>

		<nav>
      <button class="ml-3 px-60">
				<img
					src="@/assets/images.jpeg"
					alt="User Avatar"
					class=" h-9 w-9 rounded-full"
					width="36"
					height="36"
				/>
			</button>
			<button class="ml-3">
				<img
					src="@/assets/profile_.png"
					alt="User Avatar"
					class=" h-9 w-9 rounded-full"
					width="36"
					height="36"
				/>
			</button>
		</nav>
	</header>
</template>