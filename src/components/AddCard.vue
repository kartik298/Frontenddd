<template>
	<TransitionRoot appear :show="isOpen" as="template">
		<Dialog as="div" @close="emit('closeModal')">
			<div class="fixed inset-0 z-10 overflow-y-auto">
				<div class="min-h-screen px-4 text-center">
					
					<span class="inline-block h-screen align-middle" aria-hidden="true"> &#8203; </span>

					<div class="py-50">
						<div
							class="my-8 inline-block w-full max-w-md transform overflow-hidden rounded-2xl bg-white p-6 text-left align-middle shadow-xl transition-all">
							<Div as="h3" class="text-lg font-medium leading-6 text-gray-900">
								{{ isEdit ? "Edit Task" : "Add Task" }}
							</Div>
							<div class="mt-4">
								<form action="" @submit.prevent="isEdit ? updateCard() : addNewCard()" class="w-full">
									<div>
										<label class="mb-2 block text-sm font-bold text-gray-700" for="title">
											Title
										</label>
										<input v-model="task.title" type="text" name="title" autocomplete="off"
											placeholder="Enter Title..." class="form-input mb-2 w-full rounded" />
									</div>

									<div>
										<label class="mb-2 block text-sm font-bold text-gray-700" for="description">
											Description
										</label>
										<textarea v-model="task.description" name="description"
											placeholder="Enter Description..." class="form-input mb-2 w-full rounded" />
									</div>

									<div>
										<label class="mb-2 block text-sm font-bold text-gray-700" for="date">
											Image link
										</label>
										<input v-model="task.img" name="imageLink" type="text"
											placeholder="Enter Image Link" class="form-input w-full rounded" />
									</div>

									<p class="mt-4 text-center text-sm text-red-500" v-if="error">
										{{ error }}
									</p>

									<div class="mt-4">
										<div>
											<button type="button"
												class="float-left inline-flex justify-center rounded-md border border-transparent bg-red-100 px-4 py-2 text-sm font-medium text-red-900 hover:bg-red-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-red-500 focus-visible:ring-offset-2"
												@click="emit('closeModal')">
												Cancel
											</button>
										</div>
										<div>
											<button type="submit"
												class="float-right inline-flex justify-center rounded-md border border-transparent bg-green-100 px-4 py-2 text-sm font-medium text-green-900 hover:bg-green-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-green-500 focus-visible:ring-offset-2">
												{{ isEdit ? "Save" : "Create" }}
											</button>
										</div>
									</div>
								</form>
							</div>
						</div>
					</div>
				</div>
			</div>
		</Dialog>
	</TransitionRoot>
</template>

<script setup>
import { ref, reactive } from "vue";
import { TransitionRoot, Dialog } from "@headlessui/vue";
import { useBoardStore } from "@/stores/board";

const props = defineProps({
	listId: String,
	task: Object,
	isOpen: {
		type: Boolean,
		default: false,
	},
	isEdit: {
		type: Boolean,
		default: false,
	},
});

const emit = defineEmits(["onCardCreated", "closeModal"]);

const boardStore = useBoardStore();

let task = reactive({
	title: props.isEdit ? props.task.title : "",
	description: props.isEdit ? props.task.description : "",
	img: props.isEdit ? props.task.img : "",
	"avatars": [
					 "https://picsum.photos/id/237/200/300" ,
					"https://picsum.photos/200/300?grayscale"
				  ]
});

let error = ref("");

function addNewCard() {
	if (!task.title) {
		error.value = "Title field is required";
		return;
	}

	boardStore.addItem({
		listId: props.listId,
		title: task.title,
		description: task.description,
		img: task.img,
		avatars:task.avatars
	});

	task.title = "";
	task.description = "";
	task.img = "";
	error.value = "";
	emit("onCardCreated");
	emit("closeModal");
}

function updateCard() {
	if (!task.title) {
		error.value = "Title field is required";
		return;
	}

	boardStore.updateItem({
		itemId: props.task.id,
		title: task.title,
		description: task.description,
		img: task.img,
	});

	error.value = "";

	emit("closeModal");
}
</script>
