<template>
	<div @drop="onDrop($event, list.id)" class="flex max-h-full w-100 flex-col rounded-md bg-gray-800">
		<!-- List Title -->
		<div class="flex items-center justify-between px-3 py-2">
			<h3 class="text-sm font-semibold text-white">{{ list.title }}</h3>
		
		</div>

		<!-- List Cards -->
		<div class="flex flex-col overflow-hidden pb-3">
			<div ref="listRef" class="flex-1 overflow-y-auto px-3">
				<Draggable
					v-model="listItems"
					group="cards"
					item-key="id"
					class="space-y-3"
					tag="ul"
					drag-class="drag"
					ghost-class="ghost"
					@dragstart="onStart"
				>
					<template #item="{ element }">
						<TheCard :task="element" :list-id="list.id" />
					</template>
				</Draggable>
			</div>

			<!-- Add Card -->
			<div class="mt-3 px-3">
				<button
					type="button"
					@click="openModal"
					class="flex w-full items-center rounded-md p-2 text-sm font-medium text-gray-600 hover:bg-gray-300 hover:text-gray-800"
				>
					<PlusIcon class="h-5 w-5" /> <span class="ml-1"> Add Card </span>
				</button>
			</div>

			<AddCard
				v-if="isOpen"
				:is-open="isOpen"
				:list-id="list.id"
				@on-card-created="onCardCreated()"
				@close-modal="closeModal"
			/>
		</div>
	</div>
</template>

<script setup>
// General
import { ref } from "vue";
import Draggable from "vuedraggable";

// Components
import TheCard from "../components/TheCard.vue";
import AddCard from "../components/AddCard.vue";


import { PlusIcon } from "@heroicons/vue/solid";

// Store
import { useBoardStore } from "@/stores/board";
const boardStore = useBoardStore();

// Props
const props = defineProps({ list: Object });

// Data
const listItems = ref(props.list.items);

// Emits
const emit = defineEmits(["onStart", "onDrop"]);

// Modal Open/Close Methods
const isOpen = ref(false);
function openModal() {
	isOpen.value = true;
}
function closeModal() {
	isOpen.value = false;
}

// Drag n' Drop Methods
function onStart(evt) {
	emit("onStart", { evt });
}
function onDrop(evt, listId) {
	emit("onDrop", { evt, listId });
}

const listRef = ref();
function onCardCreated() {
	listRef.value.scrollTop = listRef.value.scrollHeight;
}
</script>

<style scoped>
::-webkit-scrollbar {
	width: 7px;
}
::-webkit-scrollbar-track {
	background: #5a5a5d;
	border-radius: 4px;
}
::-webkit-scrollbar-thumb {
	background: #48494e;
	border-radius: 4px;
}
</style>
