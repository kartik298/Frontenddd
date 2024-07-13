<template>
	<div class="flex h-screen flex-col bg-gray-800">
		<!-- TheNavbar -->
		<TheNavbar />

		<!-- Board Screen -->
		<main class="flex-1 overflow-hidden">
			<div class="flex h-full flex-col">
				<!-- Board Title -->
				<div class="flex shrink-0 items-center justify-between p-4">

					<div>
						<button
							class="inline-flex items-center rounded-md bg-white/10 px-3 py-2 text-sm font-medium text-white hover:bg-white/20"
						>
						</button>
					</div>
				</div>

				<div class="flex-1 overflow-x-auto">
					<div class="inline-flex h-full items-start space-x-4 px-4 pb-4">
						<!-- Board Lists -->
						<TheList
							v-for="list in lists"
							:key="list.id"
							:list="list"
							@on-start="onDragStart"
							@on-drop="onDragDrop"
						/>
					</div>
					<div>
					</div>
				</div>
			</div>
		</main>
	</div>
</template>

<script setup>
// Components
import TheNavbar from "../components/TheNavbar.vue";
import TheList from "../components/TheList.vue";


// Utils
import { getIndexOfItemById } from "../utils/board";

// Store
import { useBoardStore } from "@/stores/board";

// Data
const boardStore = useBoardStore();
const lists = boardStore.lists;

// Drag n' Drop Methods
function onDragStart({ evt }) {
	evt.dataTransfer.dropEffect = "move";
	evt.dataTransfer.effectAllowed = "move";

	evt.dataTransfer.setData("itemID", evt.srcElement.__draggable_context.element.id);
	evt.dataTransfer.setData("oldIndex", evt.srcElement.__draggable_context.index);
}

function onDragDrop({ evt, listId }) {
	const itemID = evt.dataTransfer.getData("itemID");

	const fromList = boardStore.getListByItemId(itemID);
	const oldIndex = evt.dataTransfer.getData("oldIndex");

	const toList = boardStore.getListById(listId);
	const newIndex = getIndexOfItemById(lists, itemID);

	boardStore.moveItem([fromList.id, oldIndex, toList.id, newIndex]);
}
</script>

<style scoped>

</style>
