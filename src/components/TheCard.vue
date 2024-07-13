<template>
	<li class="group relative rounded-md border-b border-gray-800 bg-white p-3 shadow hover:bg-gray-50">
	  <div class="flex flex-col">
		<!-- Image section -->
		<div v-if="task.img" class="relative w-full h-48 mb-2">
		  <img :src="task.img" :alt="'Image for ' + task.title" class="rounded-t-md object-cover w-full h-full">
		  
		</div>
  
		<!-- Title and Description section -->
		<div class="flex flex-col flex-1 py-5">
		  <h5 class="font-semibold text-gray-800 mb-2">{{ task.title }}</h5>
		</div>
	  </div>
  
	  <!-- Edit and Delete buttons on hover -->
	  <div class="absolute bottom-0 left-0 hidden group-hover:flex space-x-2 p-2">
		<button type="button" @click="openModal" class="rounded-md bg-gray-50 px-2 hover:bg-gray-100">
		  <PencilIcon class="h-6 w-6 text-gray-500" />
		</button>
		<button @click="deleteTask" class="bg-gray-50 rounded-md px-2 hover:bg-gray-100">
		  <TrashIcon class="h-6 w-6 text-red-500" />
		</button>
	  </div>
  
	  <div class="absolute bottom-0 right-0 flex space-x-2 p-2">
		<div v-for="(avatar, index) in task.avatars.slice(0, 2)" :key="index">
		  <img :src="avatar" alt="Avatar" class="h-8 w-8 rounded-full object-cover ring-2 ring-white">
		</div>
	  </div>
	  <!-- Modal for editing -->
	  <AddCard v-if="isOpen" :is-open="isOpen" :task="task" :is-edit="true" @close-modal="closeModal" />
	</li>
  </template>

<script setup>
import { ref } from "vue";
import { useBoardStore } from "@/stores/board";
import AddCard from "./AddCard.vue";
import { TrashIcon, PencilIcon } from "@heroicons/vue/solid";

import { PlusIcon } from "@heroicons/vue/solid";
const props = defineProps({ task: Object, listId: String });

const boardStore = useBoardStore();

// Modal Open/Close Methods
const isOpen = ref(false);
function openModal() {
	isOpen.value = true;
}
function closeModal() {
	isOpen.value = false;
}

// Task Methods
function deleteTask() {
	boardStore.removeItem({ itemId: props.task.id });
}
</script>

<style scoped>
.drag > div {
	transform: rotate(5deg);
}
.ghost {
	background: lightgray;
	border-radius: 6px;
}
.ghost > div {
	visibility: hidden;
}
</style>
